# Phys-434-Portfolio Lab 4

Lab 4: Working with 'real' data
Introduction
In this lab we are going to work on how to estimate the background from 'real' data. Real is in air quotes because the data is actually from simplified simulations to make the problems manageable in a single lab. But the data will have some features that resemble that of real data sets.

Getting data and HD5
In general exchanging raw data is a pain. Writing data in text files is error prone, inaccurate, and wastes space, but raw binary files have all sorts of subtleties too (including the internal byte order of your processor). To try and sidestep a whole set of nasty issues, we are going to use HDF5 (originally developed by the National Center for Supercomputing Applications for data exchange) to allow everyone to import the data.

If you are working on your own machine, in either Matlab or python, follow the links in the assignment to download the files put them in your working directory.

Python
If you are using python on your own machine, you will need to install the h5py library. Go to your Anaconda environment (if you followed the suggestions in the first lab) and search for the h5py library. Install that library into your environment, then restart your jupyter Lab or notebook session.

If you are working in the cloud python the necessary library is already installed, but you need to follow a magic incantation to download the file from the course website to your cloud instance. From the website find and copy the link address to the data file (often this means right or option-clicking on the link). Then open the terminal in your cloud instance, navigate to your working directory, and use the following command structure

wget -O <output_file_name> <link>

So my command (your link might be different) for the first file is

wget -O gammaray_lab4.h5 https://canvas.uw.edu/courses/1401649/files/67789336/download?wrap=1

Which I can see with ls -lh has downloaded a 792 MB file to my directory named gammaray_lab4.h5

Now start your lab with an import block that looks like:

%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import scipy
from scipy import stats
import h5py

#this sets the size of the plot to something useful
plt.rcParams["figure.figsize"] = (20,15)
Now import the file for problem 1 using

hf = h5py.File('gammaray_lab4.h5', 'r')
Within hdf5 files you can store different kinds of data sets. Ours is simple and has one called 'data'. You can look at the header and see this using

hf.keys()
<KeysViewHDF5 ['data']>
You can then import the data into an array variable using the get method

data = np.array(hf.get('data'))
Check that your data is as expected, with a time (in gps seconds), Solar phase (deg), Earth longitude (deg), and gamma-ray counts, and more than 25 million data records. Printing the first row you should get:

data[:,0]
array([9.40680016e+08, 3.15000000e+02, 4.50000000e+01, 1.00000000e+01])
You can then close your file

hf.close()
MatLab
In MatLab it is as usual a bit easier. First look at the file

h5disp("gammaray_lab4.h5")

Then import the data from the data 'directory' withing the hdf5 file

mydata = h5read("gammaray_lab4.h5",'/data')

and check you got the data imported (python and matlab have different row/column conventions)

mydata(1,:)

Problem 1
In this problem we are looking at the data from a gamma-ray satellite orbiting in low Earth orbit. It takes a reading of the number of particles detected every 100 milliseconds, and is in an approximately 90 minute orbit. While it is looking for gamma-ray bursts, virtually all of the particles detected are background cosmic rays.

As with most data, there are 'features.' Your lab instructor has helpfully incorporated the meta-data into your data file.

1) Down load the data from the course website (gammaray_lab4.h5), and import it into your working environment. The data has 4 columns and more than 25 million rows. The columns are time (in gps seconds), Solar phase (deg) showing the position of the sun relative to the orbit, Earth longitude (deg) giving the position of the spacecraft relative to the ground, and particle counts. Make a few plots, generally exploring your data and making sure you understand it. Give a high level description of the data features you see. Specifically comment on whether you see signal contamination in your data, and how you plan to build a background pdf().

2) The background is not consistent across the dataset. Find and describe as accurately as you can how the background changes.

3) Create a model for the background that includes time dependence, and explicitly compare your model to the data. How good is your model of the background?

4) Because the background varies, your discovery sensitivity threshold (how many particles you would need to see) also varies. What is the '5-sigma' threshold for a 100 millisecond GRB at different times?

Optional: while this is simulated data, it is based on a real effect seen by low Earth orbit satellites. Can you identify the cause of the variable background and propose a physical model?

Problem 2
In this problem we are going to look at a stack of telescope images (again simulated). We have 10 images, but you and your lab partner will be looking for different signals. One of you will be looking for the faintest stars, while the other will be looking for a transient (something like a super novae that only appears in one image). Flip a coin to determine which of you is pursuing which question.

1) Dowload the data from images.h5. This is a stack of 10 square images, each 200 pixels on a side.

2) Explore the data. Is there signal contamination? Is the background time dependent? Is it consistent spatially? Develop a plan to calculate your background pdf().

3) Using your background distribution, hunt for your signal (either faint stars, or a transient). Describe what you find.

4) You and your lab partner had different pdf(), but were using the same data. Explore why this is.