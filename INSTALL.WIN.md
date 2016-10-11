 - [ ] 1. install perl (https://www.perl.org/)
 - [ ] 2. install nasm (http://www.nasm.us/)
 - [ ] 3. add nasm in PATH (C:\Users\dev\AppData\Local\NASM)
 - [ ] 4. run Command Promt (vcvarsall.bat)

 - [ ] 5. set the build options
```bash
> set BUILD_TYPE=Release # Release | Debug
> set BUILD_SHARED_LIBS=FALSE # FALSE | TRUE
> set OPENSSL_BUILD_PLATFORM=VC-WIN64A # VC-WIN32 | VC-WIN64A | VC-WIN64I | VC-CE

> set CMAKE_COMMON_FLAGS=-DCMAKE_BUILD_TYPE=%BUILD_TYPE% -DMSVC_SHRED_RT:BOOL=%BUILD_SHARED_LIBS%
```

 - [ ] 6. download repo
```bash
> git clone https://designerror/webdav-client-cpp.git
> cd webdav-client-cpp
```

 - [ ] 7. download requirements

```bash
> mkdir build
> set INSTALL_PREFIX=%cd%\build
> git submodule update --init
```

 - [ ] 8. build and local install openssl

```bash
> cd vendor\openssl/
> mkdir build && cd build
> perl ../Configure --prefix=%INSTALL_PREFIX% --openssldir=%INSTALL_PREFIX%\ssl %OPENSSL_BUILD_PLATFORM% no-shared no-idea no-unit-test
> nmake
> nmake install
> cd ../..
```

 - [ ] 9. build and local install curl

```bash
> cd curl
> mkdir build && cd build
> cmake .. -DCMAKE_INSTALL_PREFIX=%INSTALL_PREFIX% -DCURL_STATICLIB:BOOL=ON -DBUILD_TESTING:BOOL=OFF -DBUILD_CURL_TESTS:BOOL=OFF -DBUILD_CURL_EXE:BOOL=OFF -DCURL_DISABLE_LDAP:BOOL=ON -DCURL_DISABLE_LDAPS=ON %CMAKE_COMMON_FLAGS%
> nmake
> nmake install
> cd ../..
```

 - [ ] 10. build and local install pugixml

```bash
> cd pugixml
> mkdir build && cd build
> cmake .. -DCMAKE_INSTALL_PREFIX=%INSTALL_PREFIX% %CMAKE_COMMON_FLAGS% 
> nmake
> nmake install
> cd ../..
```

 - [ ] 11. build and local install webdav-client-cpp

```bash
> cd %INSTALL_PREFIX%
> set CURL_INCLUDE_DIR=%INSTALL_PREFIX%\include
> set CURL_LIBRARY=%INSTALL_PREFIX%\lib\libcurl.lib
> set CMAKE_PUGIXML_FLAGS=-Dpugixml_LIBRARIES=%INSTALL_PREFIX%\lib\pugixml.lib -Dpugixml_INCLUDE_DIRS=%INSTALL_PREFIX%\include
> set CMAKE_CURL_FLAGS=-DCURL_STATICLIB:BOOL=TRUE -DCURL_INCLUDE_DIR:STRING=%CURL_INCLUDE_DIR% -DCURL_LIBRARY=%CURL_LIBRARY%
> set CMAKE_OPENSSL_FLAGS=-DOPENSSL_LIBRARIES=%INSTALL_PREFIX%\lib\libcrypto.lib;%INSTALL_PREFIX%\lib\libssl.lib 
> cmake .. %CMAKE_PUGIXML_FLAGS% %CMAKE_CURL_FLAGS% %CMAKE_OPENSSL_FLAGS% %CMAKE_COMMON_FLAGS%
> nmake
> nmake install
```