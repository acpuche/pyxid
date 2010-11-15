Python library for interfacing with Cedrus XID devices

XID (eXperiment Interface Device) devices are used in software such as
SuperLab, Presentation, and ePrime for receiving input as part of
stimulus/response testing experiments.

This handles all of the low level device handling for XID devices in
python projects.  The developer using this library must poll the
attached device(s) for responses.  Heres's an example of how to do so:

    import pyxid

    # get a list of all attached XID devices
    devices = pyxid.get_xid_devices()

    dev = devices[0] # get the first device to use
    dev.reset_base_timer()
    dev.reset_rt_timer()

    while True:
        dev.poll_for_response()
        if dev.respone_queue_size() > 0:
            response = dev.get_next_response()
            # do something with the response


The response is a python dict with the following keys:

    pressed: True if the key was pressed, False if it was released
    key: Response pad key pressed by the subject
    port: Device port the response was from (typically 0)
    time: value of the Response Time timer when the key was hit/released


Timers

Each Cedrus XID device has an internal timer a Base Timer and a
Response Time Timer.  The Base Timer should be reset at the start of
an experiment.  The Response Time timer should be reset whenever a
stimulus is presented.

Note: There is some known drift in the timers. 