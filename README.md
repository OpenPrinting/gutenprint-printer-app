## THE ROCK (OCI CONTAINER IMAGE)

### Install from Docker Hub
#### Prerequisites

1. **Docker Installed**: Ensure Docker is installed on your system. You can download it from the [official Docker website](https://www.docker.com/get-started).
```sh
  sudo snap install docker
```

#### Step-by-Step Guide

The first step is to pull the gutenprint-printer-app Docker image from Docker Hub.
```sh
  sudo docker pull openprinting/gutenprint-printer-app
```

Run the following Docker command to run the gutenprint-printer-app image:
```sh
  sudo docker run --rm -d \
      --name gutenprint-printer-app \
      --network host \
      -e PORT:<port> \
      openprinting/gutenprint-printer-app:latest
```
- `PORT` is an optional environment variable used to start the printer-app on a specified port. If not provided, it will start on the default port 8000 or, if port 8000 is busy, on 8001 and so on.
- **The container must be started in `--network host` mode** to allow the Printer-Application instance inside the container to access and discover printers available in the local network where the host system is in.
- Alternatively using the internal network of the Docker instance (`-p <port>:8000` instead of `--network host -e PORT:<port>`) only gives access to local printers running on the host system itself.

### Setting Up and Running gutenprint-printer-app locally

#### Prerequisites

**Docker Installed**: Ensure Docker is installed on your system. You can download it from the [official Docker website](https://www.docker.com/get-started) or from the Snap Store:
```sh
  sudo snap install docker
```

**Rockcraft**: Rockcraft should be installed. You can install Rockcraft using the following command:
```sh
  sudo snap install rockcraft --classic
```

**Skopeo**: Skopeo should be installed to compile `*.rock` files into Docker images. It comes bundled with Rockcraft, so no separate installation is required.

#### Step-by-Step Guide

**Build gutenprint-printer-app rock**

The first step is to build the Rock from the `rockcraft.yaml`. This image will contain all the configurations and dependencies required to run gutenprint-printer-app.

Open your terminal and navigate to the directory containing your `rockcraft.yaml`, then run the following command:

```sh
  rockcraft pack -v
```

**Compile to Docker Image**

Once the rock is built, you need to compile docker image from it.

```sh
  sudo rockcraft.skopeo --insecure-policy copy oci-archive:<rock_image> docker-daemon:gutenprint-printer-app:latest
```

**Run the gutenprint-printer-app Docker Container**

```sh
  sudo docker run --rm -d \
      --name gutenprint-printer-app \
      --network host \
      -e PORT:<port> \
      gutenprint-printer-app:latest
```
- `PORT` is an optional environment variable used to start the printer-app on a specified port. If not provided, it will start on the default port 8000 or, if port 8000 is busy, on 8001 and so on.
- **The container must be started in `--network host` mode** to allow the Printer-Application instance inside the container to access and discover printers available in the local network where the host system is in.
- Alternatively using the internal network of the Docker instance (`-p <port>:8000` instead of `--network host -e PORT:<port>`) only gives access to local printers running on the host system itself.

#### Setting up

Enter the web interface

```sh
http://localhost:<port>/
```

Use the web interface to add a printer. Supply a name, select the
discovered printer, then select make and model. Also set the installed
accessories, loaded media and the option defaults. If the printer is a
PostScript printer, accessory configuration and option defaults can
also often get polled from the printer.
