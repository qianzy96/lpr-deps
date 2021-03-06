# OpenALPR ARM Docker Build

Build OpenALPR and all its dependencies for ARM architecture using Docker.


## Releases

If you just want to install OpenALPR on a Raspberry Pi, you can grab the .deb files which are available on the [releases page](https://github.com/marktheunissen/lpr-deps/releases). Download them and then do:

    dpkg -i opencv_<version>_armhf.deb
    dpkg -i leptonica_<version>_armhf.deb
    dpkg -i openalpr_<version>_armhf.deb
    dpkg -i tesseract_<version>_armhf.deb
    sudo ldconfig

You'll also need to do:

    sudo apt install libcurl4-openssl-dev liblog4cplus-dev

You can then run the test:

    wget -q http://plates.openalpr.com/ea7the.jpg
    alpr -c us ea7the.jpg


## Building yourself

Either use a Raspberry Pi or fire up a Scaleway ARM cloud VM to build. You then can install the latest Docker version which is packaged by Hypriot:

    wget https://downloads.hypriot.com/docker-hypriot_1.10.3-1_armhf.deb
    sudo dpkg -i docker-hypriot_1.10.3-1_armhf.deb
    systemctl start docker

Then, build them in order, which is:

1. leptonica
2. tesseract
3. opencv
4. openalpr

You'll have to modify each Dockerfile to point to your newly built intermediate .deb files.

    cd leptonica / tesseract / etc.
    make build
    make deb

The `test` directory contains a Dockerfile that pulls in the final debs and allows you to manually verify the install worked. You can build and run it to get the OpenALPR results on the test plate:

    make build
    make run


## TODO

- Put the debs into a proper repository like Package Cloud or Launchpad PPA (which doesn't support ARM at the moment).


## Notes

This uses `fpm` to build deb files, see:

- https://github.com/jordansissel/fpm/wiki/PackageMakeInstall
