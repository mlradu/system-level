.. collection:: Hands-on Activities
   :subtitle: Kuiper hands-on exercises for the Sales 2026 workshop.
   :label: workshop

   Hands-on exercises covering device tree generation, Kuiper image building,
   SD card flashing, boot configuration, and running examples.

   system-level:
     - .

Hands-on Activities
===============================================================================

Hardware Setup
-------------------------------------------------------------------------------


Ensure you have the following hardware available:


Exercise 1: Generate a DTB with Analog Attach
-------------------------------------------------------------------------------

In this exercise, you will generate a Device Tree Blob (DTB) using
Analog Attach.

Steps
~~~~~


Exercise 2: Build a Kuiper Image using Kuiper Imager
-------------------------------------------------------------------------------

In this exercise, you will use Kuiper Imager to build a custom Kuiper image.

Steps
~~~~~

Using a basic Kuiper image and the Kuiper Imager tool, you can obtain a custom
Kuiper image, tailored to your needs. You can install only a selected few of the
features found in the full image. You can even run a personalized script to
prepare your environment for your desired activities.

Let's open Kuiper Imager first! You can find it in the Application menu in the
top right corner of your screen.

Take a moment to understand the layout the Kuiper Imager.

By clicking the "Download release" button you will enter the Release Menu where
you can find the release variants that were presented in the introduction.

In the future, you can use this menu to automatically download the release image
you need. For the scope of this workshop, we already downloaded one for you :D.

Let's close the Release Menu.

You can find the arm32 basic Kuiper image at the following path:

.. code-block:: bash

  /home/analog/image_kuiper_32_basic_2026.1.0.img

This image is obtained by downloading the 2026.1.0 32 basic Kuiper release image
and unzipping the artifact.

Now let's select the basic Kuiper image that we want to customize.

<basic_image_selcted_image>

Clicking the "Customize" button should open the Customization Menu.

<customization_menu_image>

Now let's start the custom Kuiper build by pressing the button:

We got the custom image with only the libiio feature installed in just a few
minutes. If we were to build it from scratch using the other methods presented,
it would have taken at least 20 minutes.

We can close the Customization Menu.

The custom build is being done in place, so we can find the created image at the
same path:

.. code-block:: bash

  /home/analog/image_kuiper_32_basic_2026.1.0.img

Exercise 3: Flash Kuiper on SD Card using Kuiper Imager
-------------------------------------------------------------------------------

In this exercise, you will flash the Kuiper image onto an SD card using
Kuiper Imager.

Steps
~~~~~

After making sure we have the custom image we just created selected as the
input and that we have the SD card (/dev/sda) selected as the target drive,
we can proceed with the flash.

<image_with_flash_setup>

It is VERY important here to unselect the "Verify after writing" checkbox.
Usually, it is recommended to leave it checked in order to catch any card
writing errors. But for this time-constrained workshop we will skip the Verify.

<image_with_deselcted_verify>

The flash is going to take around 10 minutes because of the hardware speed
limitations :(.

But, let's take this time to

 - Reflect on the (short) journey to a custom image just for us
 - Check out the Kuiper and Kuiper Imager documentations
 - Talk to our colleagues about Kuiper :D
 - Ask the presenters any possbile questions we might have

Amazing! We have our very own tailored Kuiper image written on the SD card.

Make sure to not remove the SD card from the adapter yet! We will need it
attached for the following exercise.

Exercise 4: Configure Boot Partition using Kuiper Imager
-------------------------------------------------------------------------------

In this exercise, you will configure the boot partition using Kuiper Imager.

Steps
~~~~~

The custom Kuiper image is written on the card, and it would be ready to boot,
if the target would be a Raspberry Pi. But, we want to boot a CoraZ7S zynq
right now.

The 32 basic image comes pre-packaged with boot files for this platform, and for
many others. We only need to configure them.

Let's open the Configuration Menu by clicking on the "Configure" button in the
drive space.

<image_configure_button>

Now you should see a list of all the projects whose boot files are integrated
in the Kuiper image.

Let's select one for platform "coraz7s" and click the "Apply" button.

<image with cora-z7 project selected>

Now, the SD card is ready to power the CoraZ7S board in front of you.
After safely ejecting it let's take it out of the SD card adapter.

<safe_eject_photo>

Let's insert it in the Cora board, power it on and wait for it to boot.

<cora boot instructions and connect and images>

With the following command we can see all the iio devices on the target board.

.. code-block:: bash

  iio_info

Take a minute and check for the ADXL355 that is attached to the Cora board.
Can you find it?

Of course we cannot find it! We didn't add the devicetree we generated during
Exercise 1 on the SD card :(.

There is a difference between configuring the standard projects that come
included in the release image, like the one we selected at configure time, and
custom boot files we create ourselves, like the one we just created using Analog
Attach.

Let's turn down the Cora board for now:

.. code-block:: bash

  sudo poweroff

And take out the SD card out of the Cora board. Then, insert it back into the
SD card adapter connected to the Raspberry Pi 5 workstation.

Let's reopen the Configuration Menu of Kuiper Imager. And click "Local files":

<image with configuration menu>

We need to select the devicetree built during Exercise 1 and the custom BOOT.bin
we prepared beforehand.

<image with local files: devicetree and bootbin configured>

Let's click the "Configure" button, once again.

<image with configuration menu>

And now the SD card is prepared for the CoraZ7S - ADXL355 setup in front of you.

Let's safely reject the SD card.

<desktop_safe_reject_image>

And we can remove the SD card from the adapter. Then, insert in back into the
target Cora. We can power it on and press the "RST" button on the board.

<cora board photo>

Let's connect to the Cora again:

<connect commands (picocom or ssh)>

And if we check for the ADXL355 one more time:

.. code-block:: bash

  iio_info

We should be able to find it. In the next exercise we will put our hand-build
setup to test.

Exercise 5: Running an Example
-------------------------------------------------------------------------------

In this exercise, you will run an example to verify the setup.

Steps
~~~~~

