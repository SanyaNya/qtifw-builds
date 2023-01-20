FROM mcr.microsoft.com/windows/servercore:ltsc2019

SHELL ["cmd", "/S", "/C"]

RUN @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" \
		-NoProfile \
		-InputFormat None \
		-ExecutionPolicy Bypass \
		-Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; \
				iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))" && \
				SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

RUN choco install strawberryperl python 7zip wget git -y

RUN wget --no-check-certificate "https://aka.ms/vs/17/release/vs_buildtools.exe" && \
	\
	vs_buildtools.exe \
		--locale en-US \
		--quiet \
		--norestart \
		--wait \
		--nocache \
		--includeRecommended   \
		--remove Microsoft.VisualStudio.Component.Windows11SDK.22000 \
		--add Microsoft.VisualStudio.Component.Windows10SDK.18362 \
		--add Microsoft.VisualStudio.Workload.VCTools && \
	\
	del vs_buildtools.exe

RUN git clone --depth=1 https://code.qt.io/qt/qt5.git --branch 5.15.2 qt5 && \
	git config --global --add safe.directory C:/qt5 && cd qt5 && perl init-repository --module-subset=qtbase,qtdeclarative,qttools,qttranslations,qtwinextras && \
	cd .. && \
	call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat" amd64 && \
	mkdir qt-build && \
    cd qt-build && \
    ..\qt5\configure.bat -opensource -confirm-license \
		-prefix "C:/Qt/Tools/QtInstallerFramework/4.5/x86_64" -release -static -static-runtime -accessibility -no-icu -no-sql-sqlite -no-qml-debug -nomake examples -nomake tests && \
    nmake module-qtbase module-qtdeclarative module-qttools module-qttranslations module-qtwinextras && \
    nmake install && \
    cd .. && rmdir /Q /S qt-build && \
	\
	call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat" x86 && \
	mkdir qt-build && \
    cd qt-build && \
    ..\qt5\configure.bat -opensource -confirm-license \
		-prefix "C:/Qt/Tools/QtInstallerFramework/4.5/i686" -release -static -static-runtime -accessibility -no-icu -no-sql-sqlite -no-qml-debug -nomake examples -nomake tests && \
    nmake module-qtbase module-qtdeclarative module-qttools module-qttranslations module-qtwinextras && \
    nmake install && \
    cd .. && rmdir /Q /S qt-build && \
	\
	rmdir /Q /S C:\qt5
	

RUN git clone http://code.qt.io/cgit/installer-framework/installer-framework.git --branch 4.5 qtifw64 && \
	git config --global --add safe.directory C:/qtifw64 && \
	\
	call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat" amd64 && \
	cd qtifw64 && \
	C:\Qt\Tools\QtInstallerFramework\4.5\x86_64\bin\qmake && nmake && nmake install && \
	cd .. && rmdir /Q /S qtifw64
	

RUN git clone http://code.qt.io/cgit/installer-framework/installer-framework.git --branch 4.5 qtifw32 && \
	git config --global --add safe.directory C:/qtifw32 && \
	\
	call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat" x86 && \
	cd qtifw32 && \
	C:\Qt\Tools\QtInstallerFramework\4.5\i686\bin\qmake && nmake && nmake install && \
	cd .. && rmdir /Q /S qtifw32
