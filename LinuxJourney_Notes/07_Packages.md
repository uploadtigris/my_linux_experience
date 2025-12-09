Software Distribution
- supply chain
	- upstream providers
		- developers who write software
	- package maintainers
		- when a new version is ready, upstream providers send it to package maintainers. These maintainers review, manage, and distribute the software to end-users in the form of packages tailored for specific Linux distributions
- common package formats
	- while software can be directly installed from source, using a package manager is far more common and efficient
	- Debian (.deb)
	- Red Hat Package Manager (.rpm)

Package Repositories
- What is it?
- How repositories work
- Configuring repository sources
	- apt sources list --> machine's package manager reads this to find which repositories to check for available software and updates
	- 

tar & gzip
- archiving vs. compression
	- archiving --> tar
		- combining multiple files & directories into a single file
	- compression --> gzip
		- process of reducing the size of a file to save disk space and speed up transfers
	- often, these two are used together
- compression single files

```bash
# compress
gzip myfile.txt

# uncompress
gunzip myfile.txt
```

- creative archives

```bash
# create archive
tar cvf myarchive.tar file1 file2 directory1
# c --> create new archive
# v --> verbose mode, which lists the files as they are processed
# f --> file, which specifies that the next argument is the name of the archive file
```

- The true power comes with combining tar and gzip
	- tar --> combine files
	- gzip --> compress the tar with gzip into a .tar.gz file

```bash
tar czvf myarchive.tar.gz file1 file2 directory1
# z --> use gzip for compression
```

- extracting files
```bash
tar xzvf myarchive.tar.gz
# x --> extract files from an archive
# z --> decompress the archive using g**z**ip
# v --> verbose mode, listing files as they are extracted
# f --> file, specifying the archive file to extract
```

Package Dependencies
- "Broken Packages" --> dependencies are missing, software installation fails
	- Shared libraries --> shared collection of pre-compiled code that multiple programs can use simultaneously

rpm and dpkg
- just like .exe is a single executable, so is .rpm or .deb
- **low-level package managers**
	- directly install, remove, or query a local package file

```bash
# Install a package
debian: $ dpkg -i some_deb_package.deb
rpm: $ rpm -i some_rpm_package.rpm
# i stands for install. you can also use the longer --install
```

```bash
# Remove a package
debian: $ dpkg -r some_deb_package.deb # --> r for remove
rpm: $ rpm -e some_rpm_package.rpm # --> e for erase
```

```bash
# list installed packages
debian: $ dpkg -l #--> list 
rpm: $ rpm -qa # --> query all
```

yum and apt
- **High-Level --> Package Management Systems**
	- manage repos, resolve dependencies, download packages, and call the low level tools underneath
- 

![[Pasted image 20251209162202.png]]

Compile Source Code