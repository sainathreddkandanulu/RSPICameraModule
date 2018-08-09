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

Load `v4l2loopback` module via

    sudo modprobe v4l2loopback video_nr=1 # creates /dev/video1
    # sudo modprobe v4l2loopback devices=1 

or add `v4l2loopback video_nr=1` to `/etc/modules`.