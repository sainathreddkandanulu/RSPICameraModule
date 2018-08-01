# Enable camera module

Edit your /boot/config.txt file and make sure the following lines look like this:

    start_x=1             # essential
    gpu_mem=128           # at least, or maybe more if you wish
    disable_camera_led=1  # optional, if you don't want the led to glow

Load `bcm2835-v4l2` module

    sudo modprobe bcm2835-v4l2

or add `bcm2835-v4l2` to `/etc/modules`.