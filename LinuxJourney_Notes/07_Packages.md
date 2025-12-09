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

Package Dependencies

rpm and dpkg

yum and apt

Compile Source Code