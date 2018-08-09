# Enable camera module

Edit your /boot/config.txt file and make sure the following lines look like this:

    start_x=1             # essential
    gpu_mem=128           # at least, or maybe more if you wish
    disable_camera_led=1  # optional, if you don't want the led to glow

Load `bcm2835-v4l2` module

    sudo modprobe bcm2835-v4l2

or add `bcm2835-v4l2` to `/etc/modules`.

# Create loopback device

The video device `/dev/video0` can only be accessed by one process at a time. Use a loopback device to allow mutli process access at `/dev/video1`. https://raspberrypi.stackexchange.com/a/19897

## Install v4l2loopback

    # Install kernel headers
    # Note: Kernel headers may not be up-to-date. 
    # To downgrade the firmware to a specific firmware use sudo rpi-update <commit hash from https://github.com/Hexxeh/rpi-firmware>
    sudo apt-get install raspberrypi-kernel-headers
    
    # Download source and install
    git clone https://github.com/umlaeute/v4l2loopback
    cd v4l2loopback
    make
    sudo make install

Load `v4l2loopback` module via

    sudo modprobe v4l2loopback video_nr=1 # creates /dev/video1
    # sudo modprobe v4l2loopback devices=1 

If you want to load the module on startup add `v4l2loopback` to `/etc/modules` and the options to `/etc/modprobe.d/v4l2loopback.conf`. https://askubuntu.com/a/822136

    # /etc/modprobe.d/v4l2loopback.conf
    options v4l2loopback video_nr=1

Then copy the video from `/dev/video0` to `/dev/video1`.

    ffmpeg -f video4linux2 -i /dev/video0 -vcodec copy -f v4l2 /dev/video1