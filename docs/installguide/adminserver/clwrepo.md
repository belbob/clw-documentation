# clw-repo
Created Tuesday 24 March 2015

### install createrepo

``# yum install createrepo``

### create directory

``# mkdir -p /share/CentOS/6/clw/i386/RPMS``

``# mkdir -p /share/CentOS/6/clw/x86_64/RPMS``

``# mkdir -p /share/CentOS/7/clw/i386/RPMS``

``# mkdir -p /share/CentOS/7/clw/x86_64/RPMS``

### get files and unpack

	# wget -c\
	http://download.documentfoundation.org/libreoffice/stable/4.4.1/rpm/x86/\
	        LibreOffice_4.4.1_Linux_x86_rpm.tar.gz
	http://download.documentfoundation.org/libreoffice/stable/4.4.1/rpm/x86/\
	        LibreOffice_4.4.1_Linux_x86_rpm_langpack_nl.tar.gz\
	http://download.documentfoundation.org/libreoffice/stable/4.4.1/rpm/x86/\
	        LibreOffice_4.4.1_Linux_x86_rpm_helppack_nl.tar.gz``

``# tar -xzvf LibreOffice_4.4.1_Linux_x86_rpm.tar.gz``

``# tar -xzvf LibreOffice_4.4.1_Linux_x86_rpm_langpack_nl.tar.gz``

``# tar -xzvf LibreOffice_4.4.1_Linux_x86_rpm_helppack_nl.tar.gz``

### Move the files to the repo and create metadata

``# cp LibreOffice_4.4.1.2_Linux_x86_rpm/RPMS/* /share/CentOS/6/clw/i386/RPMS/``

``# cp LibreOffice_4.4.1.2_Linux_x86_rpm_langpack_nl/RPMS/* /share/CentOS/6/clw/i386/RPMS/``

``# cp LibreOffice_4.4.1.2_Linux_x86_rpm_helppack_nl/RPMS/* /share/CentOS/6/clw/i386/RPMS/``

``# createrepo /share/CentOS/6/clw/i386``

``# chmod -R o-w+r /share/CentOS/6/clw``

#### After new packages are added to the repo

``# createrepo --update /share/CentOS/6/clw/i386``

``# chmod -R o-w+r /share/CentOS/6/clw``

### Repo using http
Create a symbolic link in the default Apache root directory to our new repository. We’re going to create the link so it points to the root of our CentOS repo directory. This lessens the effort required when adding new releases to the repo.

``# ln -s /var/www/html/CentOS /share/CentOS``

### Create /etc/yum.repos.d/clw.repo

	# cat << EOT > /etc/yum.repos.d/clw.repo
	[clw]
	name=CentOS-$releasever - clw packages for $basearch
	baseurl=http://192.168.225.3/CentOS/$releasever/clw/$basearch
	enabled=1
	gpgcheck=0
	EOT



