Bootstrap: yum
OSVersion: 7
MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum
Stage: build

%environment
    export LC_ALL=C
    export INSTALL_PATH=/usr/local
    export PATH=/usr/local/bin:/usr/local/gcc-10/bin:$PATH
    export PERL_MM_USE_DEFAULT=1
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib64:/usr/lib64:/usr/local/lib

%post
    NOW=`date`
    echo "export NOW=\"${NOW}\"" >> $SINGULARITY_ENVIRONMENT
    echo "export PATH=/bin:/sbin:/usr/local/bin:/usr/local/gcc-10/bin:$PATH" >> $SINGULARITY_ENVIRONMENT
    echo "export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib64:/usr/lib64:/usr/local/lib " >> $SINGULARITY_ENVIRONMENT
    yum -y update
    yum -y install epel-release
    yum -y install perl perl-App-cpanminus \
    	vim wget make tar gzip bzip2 gsl gsl-devel texinfo \
	glibc-devel.i686 glibc-devel vim flex \
    	wget make tar gzip bzip2 gsl texinfo hostname \
    	mercurial openssh-clientsblas blas-devel lapack gcc-c++ \
        file expat-devel perl-XML* patch gmp gmp-devel git \
        zlib-devel gcc mercurial openssh-clients gfortran \
	centos-release-scl
    yum -y install devtoolset-8
    cpanm -i Config::Tiny YAML Cwd DateTime \
	LaTeX::Encode NestedMap Scalar::Util \
	Data::Dumper Term::ANSIColor Module::New::File::Changes \
	Text::Table Sort::Topological Text::Template \
	Sort::Topological List::Uniq Regexp::Common \
	XML::Validator::Schema List::MoreUtils \
	File::Copy File::Slurp File::Next XML::Simple \
	XML::SAX::Expat XML::SAX::ParserFactory 
    # scl enable devtoolset-8 bash
    
    # install ISL
    cd /opt
    wget https://gcc.gnu.org/pub/gcc/infrastructure/isl-0.15.tar.bz2
    tar xjvf isl-0.15.tar.bz2
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib64:/usr/lib64:/usr/local/lib
    cd isl-0.15
    ./configure 
    make 
    make install
    
    # install GFortran
    cd /usr/local
    if [ -f /usr/local/gcc-10-20200308.tar.xz ] ; then 
        echo "Download Done"
    else
        wget http://gfortran.meteodat.ch/download/x86_64/snapshots/gcc-10-20200308.tar.x
    fi
    tar xvfJ gcc-10-20200308.tar.xz
    export PATH=/bin:/usr/local/gcc-10/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/gcc-10/lib:/usr/local/gcc-10/lib64:$LD_LIBRARY_PATH
    echo CHECKING WHICH GFORTRAN
    which gfortran
    gfortran -v
    
    # install FGSL v0.9.4
    cd /opt
    wget https://www.lrz.de/services/software/mathematik/gsl/fortran/download/fgsl-0.9.4.tar.gz
    tar -vxzf fgsl-0.9.4.tar.gz
    cd fgsl-0.9.4
    ./configure --gsl /usr --f90 gfortran --prefix /usr/local
    make
    make install

    # install HDF5 v1.8.20
    cd /opt
    wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.20/src/hdf5-1.8.20.tar.gz
    tar -vxzf hdf5-1.8.20.tar.gz
    cd hdf5-1.8.20
    ./configure --prefix=/usr/local --enable-fortran --enable-production
    make -j4
    make install

    # install FoX v4.1.0
    cd /opt
    wget https://github.com/andreww/fox/archive/4.1.0.tar.gz
    tar xvfz 4.1.0.tar.gz
    cd fox-4.1.0
    ./configure
    make
    make install

    # install ANN 1.1.2 (optional)
    cd /opt
    wget http://www.cs.umd.edu/~mount/ANN/Files/1.1.2/ann_1.1.2.tar.gz
    tar xvfz ann_1.1.2.tar.gz
    cd ann_1.1.2
    make linux-g++
    cp bin/* /usr/local/bin/
     
    # install Galacticus
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib64:/usr/lib64:/usr/local/lib
    export GALACTICUS_EXEC_PATH=/usr/local/galacticus/
    export GALACTICUS_DATA_PATH=/usr/local/galacticus_datasets
    cd /usr/local
    git clone https://github.com/galacticusorg/galacticus.git
    git clone https://github.com/galacticusorg/datasets.git galacticus_datasets
    cd /usr/local/galacticus
    make -j4 Galacticus.exe
     
%labels
    Author ffayton@carnegiescience.edu
    Version v0.0.1

%help
    This is Container was created as a base image for scientific programs compiled with GCC-10.
