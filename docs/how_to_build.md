KazooPopup - How to build
=========================

## Dependencies

| Name         | Version                               |
|--------------|---------------------------------------|
| Qt           | >= 5.3.0 (using websockets module)    |
| C++ compiler | supporting C++11 (i.e. gcc 4.6+)      |
| NSIS		   | >= 2.46 (for setup on Windows)		   |

## Using QtCreator

1) Build quazip library `autoupdater/3rdparty/quazip/src/quazip.pro` (see [how_to_build][how_to_build_quazip])

2) Copy library to `autoupdater/3rdparty/quazip/lib`

3) Build autoupdater application `autoupdater/autoupdater.pro`

4) Build KazooPopup application `KazooPopup.pro`

## Using terminal

* Windows
            
            1) Set Qt environment: run $$QT_DIR\bin\qtenv2.bat 
                e.g. c:\Qt\Qt5.3.1\5.3\mingw482_32\bin\qtenv2.bat
				
			2) Build quazip library
				qmake quazip.pro
				mingw32-make -jN* release
			
			3) Build autoupdater application
				qmake autoupdater.pro
				mingw32-make -jN* release
			
			4)	Build KazooPopup application
				qmake KazooPopup.pro
				mingw32-make -jN* release
				
* Mac OS
	
            1) Set Qt environment: export PATH=$QT_DIR/bin:$PATH
                e.g. export PATH=/Users/Alex/Qt5.3.0/5.3/clang_64/bin:$PATH
				
			2) Build quazip library
				qmake quazip.pro
				make -jN*
			
			3) Build autoupdater application
				qmake autoupdater.pro
				make -jN*
				make deploy
			
			4)	Build KazooPopup application
				qmake KazooPopup.pro
				make -jN*
				make deploy

## Using Jenkins

* Windows

            cd KazooPopup
            pushd .
            ; Set Qt environment
            call $$QT_DIR\bin\qtenv2.bat ;(e.g. c:\Qt\Qt5.3.1\5.3\mingw482_32\bin\qtenv2.bat)
            
            popd
            
            rd /s /q build
            mkdir build
            
            pushd build
			
			qmake ..\autoupdater\3rdparty\quazip\src\quazip.pro
			mingw32-make -jN* release ;(e.g. mingw32-make -j4 release)
			
			copy /y autoupdater\3rdparty\quazip\release\quazip.dll autoupdater\3rdparty\quazip\lib
			copy /y autoupdater\3rdparty\quazip\release\quazip.dll setup
			
			qmake autoupdater\autoupdater.pro
			mingw32-make -jN* release
			
			copy /y autoupdater\release\autoupdater.exe setup
            
            qmake ..\KazooPopup.pro
            
            mingw32-make -jN* release 
            
            copy /y release\KazooPopup.exe setup

            del setup\*.exe

            "c:\Program Files (x86)\NSIS\makensis.exe" setup\setup.nsi
            
            popd
            
* Mac OS

            cd KazooPopup
            pushd .
            # Set Qt environment
            export PATH=$QT_DIR/bin:$PATH #(e.g. export PATH=/Users/Alex/Qt5.3.0/5.3/clang_64/bin:$PATH)
            
            popd
            
            rm -rf build
            mkdir build
            
            pushd build
			
			qmake ../autoupdater/3rdparty/quazip/src/quazip.pro
			make -jN* #(e.g. make -j4)
			
			cp autoupdater/3rdparty/quazip/release/libquazip.* autoupdater/3rdparty/quazip/lib
			
			qmake autoupdater/autoupdater.pro
			make -jN*
			make deploy
            
            qmake ../KazooPopup.pro
            
            make -jN*
            
            make deploy
			
			cp autoupdater/release/autoupdater release/KazooPopup.app/Contents/MacOS
			cp autoupdater/3rdparty/quazip/release/libquazip.* release/KazooPopup.app/Contents/Framework
			
			make makedmg
            
            popd
			
\* N in -jN is the number of CPU cores on your system		

[how_to_build_quazip]: https://github.com/2600hz/kazoo-popup/blob/master/autoupdater/3rdparty/quazip/src/how_to_build.md