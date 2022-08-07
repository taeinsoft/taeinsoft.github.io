---
title: "네트워크(NIC) Enable/Disable 하기"
categories:
  - Win32API
tags:
  - Windows
  - C++
  - Win32API 
date: 2018-09-30
last_modified_at: 2021-11-02
toc: false
---

컴퓨터에서 인터넷을 차단하려면 두가지 방법이 있습니다. 첫번째는 랜선을 뽑는 방법이 있겠고, 두번째는 제어판에 들어가 네트워크 장치를 사용안함으로 설정하는 것입니다. 지금부터 소개해드리는 방법은 프로그램에서 네트워크
장치를 사용/사용안함 처리 할 수 있는 방법입니다. 전문용어로는 "NIC의 Enable / Disable 기능 구현" 쯤으로 부를 수 있을 것 같습니다.

소스코드를 한줄한줄 설명하기 위해서는 기반 지식을 먼저 소개해드려야 하기때문에 지금은 코드만 소개해 드리겠습니다. 쉬운 코드이니, 콘솔 프로젝트 생성하셔서 소스 붙여넣기 하시고, 실행하면서 Break point
걸면서 확인해보시면 바로 응용해서 사용하실 수 있을 것같습니다.

``` cpp
#include<windows.h>
#include<netcon.h>
#include<iostream>
#include<string>

using namespace std;

BOOL IsConnection();
int Connect();

int main(){
    int hr = 0;

    if(IsConnection()){
        // 연결이 되어있는 상태입니다.
        cout << "현재 네트워크 카드의 드라이브는 사용중입니다." << endl;
        cout << "연결 해제합니다.";
    }else{
        // 연결이 되어있지 않으니 연결합니다.
        cout << "현재 네트워크 카드의 드라이브는 사용중지된 상태입니다." << endl;
        cout << "연결 합니다. : ";
    }
    hr = Connect();
 
    if(hr==0){
        cout << "네트워크 카드의 연결이 해제되었습니다." << endl;
    }else if(hr == 1){
        cout << "네트워크 카드가 연결되었습니다." << endl;
    }
    return 0;
}

BOOL IsConnection(){
    int status = FALSE; //1:connect, 0:disconnect
    HRESULT result = 0;

    CoInitialize(0);
 
    INetConnectionManager *pMan = 0;
    HRESULT hres = CoCreateInstance(CLSID_ConnectionManager,
                                                        0,
                                                        CLSCTX_ALL,
                                                        __uuidof(INetConnectionManager),
                                                        (void**)&pMan);
 
    if (SUCCEEDED(hres)){
        IEnumNetConnection *pEnum = 0;
        hres = pMan->EnumConnections(NCME_DEFAULT, &pEnum);
        if (SUCCEEDED(hres)){
            INetConnection *pCon = 0;
            ULONG count;
            while (pEnum->Next(1, &pCon, &count) == S_OK ){
                NETCON_PROPERTIES *pProps = 0;
                hres = pCon->GetProperties(&pProps);
                if (SUCCEEDED(hres)){
                    if (pProps->Status == NCS_DISCONNECTED){
                        status = FALSE;
                    }
                    else if (pProps->Status == NCS_CONNECTED){
                        status = TRUE;
                    }
 
                    if (pProps){
                        CoTaskMemFree(pProps->pszwName);
                        CoTaskMemFree(pProps->pszwDeviceName);
                        CoTaskMemFree(pProps);
                    }
                }
                pCon->Release();
            }
            pEnum->Release();
        }
        pMan->Release();
    }
 
    CoUninitialize();
    return status;
}

int Connect(){
    int status = 0; //1:connect, 0:disconnect
    HRESULT result = 0;

    CoInitialize(0);
 
    INetConnectionManager *pMan = 0;
    HRESULT hres = CoCreateInstance(CLSID_ConnectionManager,
                                                        0,
                                                        CLSCTX_ALL,
                                                        __uuidof(INetConnectionManager),
                                                        (void**)&pMan);
 
    if (SUCCEEDED(hres)){
        IEnumNetConnection *pEnum = 0;
        hres = pMan->EnumConnections(NCME_DEFAULT, &pEnum);
        if (SUCCEEDED(hres)){
            INetConnection *pCon = 0;
            ULONG count;
            while (pEnum->Next(1, &pCon, &count) == S_OK){
                NETCON_PROPERTIES *pProps = 0;
                hres = pCon->GetProperties(&pProps);
                if (SUCCEEDED(hres)){
                    if (pProps->Status == NCS_DISCONNECTED){
                        result = pCon->Connect();
                        status = 1;
                    }
                    else if (pProps->Status == NCS_CONNECTED){
                        result = pCon->Disconnect();
                        status = 0;
                    }
 
                    if (pProps){
                        CoTaskMemFree(pProps->pszwName);
                        CoTaskMemFree(pProps->pszwDeviceName);
                        CoTaskMemFree(pProps);
                    }
                }
                pCon->Release();
            }
            pEnum->Release();
        }
        pMan->Release();
    }
 
    CoUninitialize();
    return status;
}
```
