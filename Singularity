Bootstrap: yum
OSVersion: 7
MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum
Stage: build

%environment
    export LC_ALL=C
    export INSTALL_PATH=/usr/local
    export PATH=/usr/local:$PATH
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib64
    export PERL_MM_USE_DEFAULT=1
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/lib64:/usr/local/lib

%post
    NOW=`date`
    echo "export NOW=\"${NOW}\"" >> $SINGULARITY_ENVIRONMENT
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/lib64:/usr/local/lib
    yum -y update
    yum -y install epel-release
    yum -y install perl perl-App-cpanminus \
    	vim wget make tar gzip bzip2 gsl texinfo \
	glibc-devel.i686 glibc-devel vim flex \
    	wget make tar gzip bzip2 gsl texinfo\
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
    scl enable devtoolset-8 bash
    
    # install GFortran
    cd /opt
    git clone git://gcc.gnu.org/git/gcc.git
    export PATH=/usr/local/bin:$PATH
    cd gcc
    ./contrib/download_prerequisites
    ./configure --prefix=/usr/local --enable-languages=c,c++,fortran --enable-multilib
    make -j2
    make install 
    
    #install GSL v1.15
    cd /opt
    wget http://ftp.gnu.org/pub/gnu/gsl/gsl-1.15.tar.gz
    tar xvfz gsl-1.15.tar.gz
    cd gsl-1.15
    ./configure --prefix=/usr/local --disable-static
    make -j4
    make check
    make install
    
    # install FGSL v0.9.4
    cd /opt
    wget https://www.lrz.de/services/software/mathematik/gsl/fortran/download/fgsl-0.9.4.tar.gz
    tar -vxzf fgsl-0.9.4.tar.gz
    cd fgsl-0.9.4
    ./configure --gsl /usr/local --f90 gfortran --prefix /usr/local
    make -j2
    make test
    make install

    # install HDF5 v1.8.20
    cd /opt
    wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.20/src/hdf5-1.8.20.tar.gz
    tar -vxzf hdf5-1.8.20.tar.gz
    cd hdf5-1.8.20
    F9X=gfortran ./configure --prefix=/usr/local --enable-fortran --enable-production
    make -j2
    make install

    # install FoX v4.1.0
    cd /opt
    wget https://github.com/andreww/fox/archive/4.1.0.tar.gz
    tar xvfz 4.1.0.tar.gz
    cd fox-4.1.0
    FC=gfortran ./configure
    make -j2
    make install

    # install FFTW 3.3.4 (optional)
    cd /opt
    wget ftp://ftp.fftw.org/pub/fftw/fftw-3.3.4.tar.gz
    tar xvfz fftw-3.3.4.tar.gz
    cd fftw-3.3.4
    ./configure --prefix=/usr/local
    make -j2
    make install

    # install ANN 1.1.2 (optional)
#    cd /opt
#    wget http://www.cs.umd.edu/~mount/ANN/Files/1.1.2/ann_1.1.2.tar.gz
#    tar xvfz ann_1.1.2.tar.gz
#    cd ann_1.1.2
#    make linux-g++
#    cp bin/* /usr/local/bin/

%labels
    Author ffayton@carnegiescience.edu
    Version v0.0.1

%help
    This is Container was created as a base image for scientific programs compiled with GCC-10.
