# ALL-deb-EXTRACTOR
this is a simple script i made to automate the task of making a full copy of all currently installed packagges on your debian os 

#!/bin/bash

# Create a new directory for the operation
mkdir -p my_package_repo
cd my_package_repo

# Get the list of installed packages
apt list --installed | awk -F '/' '{print $1}' > installed_packages.txt

# Use parallel for faster downloads
sudo apt install parallel -y

# Download .deb files (with dependencies) using parallel
cat installed_packages.txt | parallel -j $(nproc) "apt-get download $(apt-rdepends {} | grep -v '^ ')"

# Clean up any unnecessary files (if needed)
# For example, remove any .tar.gz or .tar.xz files
# find . -name "*.tar.gz" -delete
# find . -name "*.tar.xz" -delete

echo "Packages downloaded to the 'my_package_repo' directory."


this will create a new directory and and use dpkg to begain the process of repackagaing a copy of each current package version on a already installeed debian system . each package with be a fully installable .deb
