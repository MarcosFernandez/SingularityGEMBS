Bootstrap: shub
From: singularityhub/ubuntu

%labels
    Author Marcos Fernandez-Callejo
    Version v0.0
    gemBS_Version 2.0
    build_date 2018 March 8

%runscript

    exec gemBS "$@"

%post
    apt-get update && apt-get -y install python2.7 python-dev git wget
    apt-get -y install g++ build-essential
    apt-get -y install libbz2-dev liblzma-dev zlib1g-dev

    #Install HTSLIB
    wget -c https://github.com/samtools/htslib/releases/download/1.6/htslib-1.6.tar.bz2
    tar xjf htslib-1.6.tar.bz2
    cd htslib-1.6;./configure;make;make install;cd ..
    rm -fR htslib-1.6
    
    #Install SAMTOOLS
    apt-get -y install libncurses5-dev
    wget -c https://github.com/samtools/samtools/releases/download/1.6/samtools-1.6.tar.bz2
    tar xjf samtools-1.6.tar.bz2
    cd samtools-1.6;./configure;make;make install;cd ..

    #Install BCFTOOLS
    wget -c https://github.com/samtools/bcftools/releases/download/1.6/bcftools-1.6.tar.bz2
    tar xjf bcftools-1.6.tar.bz2
    cd bcftools-1.6;./configure;make;make install
    rm -fR bcftools-1.6

    #Install GSL
    wget -c ftp://ftp.gnu.org/gnu/gsl/gsl-latest.tar.gz
    tar xzf gsl-latest.tar.gz
    cd gsl-*/;./configure;make;make install;
    rm -fR gsl-*/

    #Install PYTHON modules
    apt-get -y install python-pip libfreetype6-dev libpng-dev pkg-config
    pip install --upgrade numpy
    pip install --upgrade pip
    pip install --upgrade matplotlib[mplot3d]

    #LaTeX packages
    apt-get install -y software-properties-common texlive
 
    #wigToBigWig
    wget -c http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/wigToBigWig
    chmod 777 wigToBigWig 
    cp wigToBigWig /usr/bin/

    #GEMBS
    pip install -U setuptools
    git clone --recursive https://github.com/heathsc/gemBS.git
    sed 's,-L/apps/GSL/2.4/lib/,-L/usr/local/lib,' gemBS/tools/bs_call/Gsl.mk > gemBS/tools/bs_call/Gsl.mk.1    
    sed 's,-L/apps/GSL/2.4/include/,-L/usr/local/include/gsl,' gemBS/tools/bs_call/Gsl.mk.1 > gemBS/tools/bs_call/Gsl.mk.2
    mv gemBS/tools/bs_call/Gsl.mk.2 gemBS/tools/bs_call/Gsl.mk
    rm gemBS/tools/bs_call/Gsl.mk.1
    cd gemBS;python setup.py install;cd ..
    rm -fR gemBS    
