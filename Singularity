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
    echo "export PATH=/usr/local/bin:/usr/local/gcc-10/bin:$PATH" >> $SINGULARITY_ENVIRONMENT
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
    scl enable devtoolset-8 bash
    
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
    wget http://gfortran.meteodat.ch/download/x86_64/snapshots/gcc-10-20200308.tar.xz
    tar xvfJ gcc-10-20200308.tar.xz
    export PATH=/usr/local/gcc-10/bin:$PATH
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
    
%labels
    Author ffayton@carnegiescience.edu
    Version v0.0.1

%help
    This is Container was created as a base image for scientific programs compiled with GCC-10.
