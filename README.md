# Pharmacometrics-Docker

Dockerfiles for pharmacometrics-related software: NONMEM, NMQual, and
Perl-speaks-NONMEM

Each of these files is intended to help improve reproducible research
by enabling the use of Docker images to keep all requirements for
execution in a single container.

## NONMEM 7.4.3

A dockerfile to build a gfortran-run NONMEM 7.4.3 installation.  It
will require a NONMEM license file (in the same directory nonmem.lic).
If you will be installing both NONMEM and NMQual, see the instructions
in the comments of the file for how to speed up the run (and minimize
download time).

http://www.iconplc.com/innovation/solutions/nonmem/

### Installation

* Copy your nonmem license file (named `nonmen.lic` to the same
  directory as the Dockerfile.
* Have your NONMEM zip file password handy
* See the instructions in the top of the Dockerfile for the command
  to run.
* For NONMEM, automatic download from Icon may be unreliable
  (https://github.com/billdenney/Pharmacometrics-Docker/issues/2).
  Manual download and serving the file from a local webserver is
  recommended.  (See the top of the Dockerfile for instructions.)

### Running

It is recommended to run NONMEM via Perl-speaks-NONMEM (below).  To
run NONMEM directly, you can run the following command:

    docker run --rm --user=$(id -u):$(id -g) -v $(pwd):/data -w /data humanpredictions/nonmem /opt/NONMEM/nm_current/run/nmfe CONTROL.mod CONTROL.res

### Updating Your License

To update your license file without requiring a rebuild of the Docker
image, you can mount a directory containing the license file in the
/license directory of your image (note the first -v argument):

    docker run --rm --user=$(id -u):$(id -g) -v /opt/NONMEM/license:/opt/NONMEM/nm_current/license -v $(pwd):/data -w /data humanpredictions/nonmem /opt/NONMEM/nm_current/run/nmfe CONTROL.mod CONTROL.res

## NMQual 8.4.0

A dockerfile to build a gfortran-run NONMEM 7.4.3 with NMQual 8.4.0.
It will require a NONMEM license file (in the same directory
nonmem.lic).  If you will be installing both NONMEM and NMQual, see
the instructions in the comments of the file for how to speed up the
run (and minimize download time).

https://bitbucket.org/metrumrg/nmqual/

### Running

It is recommended to run NONMEM via Perl-speaks-NONMEM (below).  To
run NONMEM directly, you can run the following command:

    docker run --rm --user=$(id -u):$(id -g) -v $(pwd):/data -w /data humanpredictions/nmqual /opt/NONMEM/nm_current/run/nmfe CONTROL.mod CONTROL.res

### Updating Your License

To update your license file without requiring a rebuild of the Docker
image, you can mount a directory containing the license file in the
/license directory of your image (note the first -v argument):

    docker run --rm --user=$(id -u):$(id -g) -v /opt/NONMEM/license:/opt/NONMEM/nm_current/license -v $(pwd):/data -w /data humanpredictions/nmqual /opt/NONMEM/nm_current/run/nmfe CONTROL.mod CONTROL.res

### Installation

* Copy your nonmem license file (named `nonmen.lic`) to the same
  directory as the Dockerfile.
* Have your NONMEM zip file password handy
* See the instructions in the top of the Dockerfile for the command
  to run.

## Perl-speaks-NONMEM

A dockerfile to build a Perl-speaks-NONMEM (PsN) 4.9.0 installation on top
of the NMQual docker image.  You must build the NMQual image first to
build the PsN image.

https://github.com/UUPharmacometrics/PsN/

### Installation

* Install the NMQual image above (this image starts from that image)
* See the instructions in the top of the Dockerfile for the command
  to run.

### Running

It is recommended to run NONMEM via the dockpsn script.  To run the
dockpsn command, set it up by copying it to a location in the path:

    cp scripts/dockpsn /usr/local/bin/dockpsn

Then you can use it by running it followed by the PsN command of
interest:

    dockpsn execute CONTROL.mod

To run PsN directly, you can use the following command (substitute
`execute` for the PsN command of interest):

    docker run --rm --user=$(id -u):$(id -g) -v $(pwd):/data -w /data humanpredictions/psn execute CONTROL.mod

### Updating Your License

If you use the `dockpsn` command, it will look for an updated license
in the `/opt/NONMEM/license` directory by default.  If none is found
there, it will run with the license used when the image was created.

To update your license file without requiring a rebuild of the Docker
image, you can mount a directory containing the license file in the
/license directory of your image (note the first -v argument):

    docker run --rm --user=$(id -u):$(id -g) -v /opt/NONMEM/license:/opt/NONMEM/nm_current/license -v $(pwd):/data -w /data humanpredictions/psn execute CONTROL.mod

That is automatically done with the `dockpsn` command.

## PMx-Rocker

A dockerfile to build R 3.3.0 with added packages from a .csv file.
This is based on the Rocker image.

https://github.com/rocker-org/rocker
https://cran.r-project.org/

### Installation

* Optionally modify the list of packages to install (see [PMxrocker/packages.csv](PMx_rocker/packages.csv))
* See the instructions in the top of the Dockerfile for the command
  to run.
