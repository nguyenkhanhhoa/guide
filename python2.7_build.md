# Build python 2.7.18 from source

wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz

tar -xzf Python-2.7.18.tgz 

cd  Python-2.7.18

nano Modules/Setup.dist 

#find zlib and uncomment line #zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz

./configure --enable-optimizations

make -s -j8

sudo make install

sudo python -m ensurepip
