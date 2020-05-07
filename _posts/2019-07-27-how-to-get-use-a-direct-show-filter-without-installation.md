---
layout: post
title:  "How to get use a DirectShow filter dll without installation"
date:   2019-07-27 18:36:48 +0900
tags: direct-show C++
---

Normally, before using a DirectShow filter dll, it has to be installed into Windows by means of regsvr32.exe. But there’s also a way to get use it without installation. The below code snippet shows how to do this.

※The source code has not been tested yet!

```cpp
#include <dshow.h>
#include <atlbase.h>

HRESULT CreateDSFilterInstance(
    LPCTSTR dll_file,
    LPCTSTR clsid_str, 
    CComPtr<IBaseFilter>& instance)
{
   HRESULT hr = S_OK; 
   GUID clsid;
   HINSTANCE hLib = NULL;
   LPFNGETCLASSOBJECT pfnGetClassObject = NULL;
   CComPtr<IClassFactory> class_factory;

   SetLastError(0);

   // Parse clsid_str
   hr = CLSIDFromString(T2COLE(clsid_str), &clsid);
   // Error
   if (FAILED(hr))
      return hr;

   // Load dll
   hLib = CoLoadLibrary(T2OLE((LPTSTR)dll_file), TRUE);
   if (NULL == hLib){
      // Error
      DWORD dwError = GetLastError();
      if (dwError != 0)
         hr = HRESULT_FROM_WIN32(dwError);
      else
         hr = HRESULT_FROM_WIN32(ERROR_INVALID_DLL);
      return hr;
   }

   // Get DllGetClassObject function pointer
   pfnGetClassObject = (LPFNGETCLASSOBJECT)GetProcAddress(hLib, "DllGetClassObject");
   if (NULL == pfnGetClassObject){
      // Error
      DWORD dwError = GetLastError();
      if (dwError != 0)
         hr = HRESULT_FROM_WIN32(dwError);
      else
         hr = HRESULT_FROM_WIN32(ERROR_INVALID_DLL);
      return hr;
   }

   // Call DllGetClassObject to get class_factory 
   hr = pfnGetClassObject(clsid, IID_IClassFactory, &class_factory );
   // Error
   if (FAILED(hr)) 
      return hr;

   hr = class_factory->CreateInstance(NULL, __uuidof(IBaseFilter), (void**)&instance);
   return hr;
}
```

Reference:

(1) <https://stackoverflow.com/questions/3560855/how-to-use-install-custom-directshow-filter>

(2) <https://github.com/cplussharp/graph-studio-next>