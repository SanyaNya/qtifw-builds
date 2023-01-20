# qtifw-builds

# Build on windows:
* docker build -t qtifw-builds windows/
* docker run qtifw-builds
* docker cp id_of_your_container:C:\Qt\Tools\QtInstallerFramework\x86_64 path_to_copy\qtifw-win64
* docker cp id_of_your_container:C:\Qt\Tools\QtInstallerFramework\i686 path_to_copy\qtifw-win32
