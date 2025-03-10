# downsample_srf
method for downsampling SRF files

# Cookbook for using method
 here is a cookbook summary of what may work (and even if it doesn’t work, you’ll see the gist of the approach).
 
Compiling new executable:
 
Copy attached srf.tgz to …./bbp-22.4.0/bbp/src/gp/StandRupFormat
Type “% tar xzvf srf.tgz”
Type “% make clean”
Type “% make srf_downsample”
 
If all goes well, the downsampling executable will now be installed in the gp/bin directory.
 
Now go to …./bbp-22.4.0/bbp/comps and add the following text after line 335 in genslip.py:
 
#
# downsample SRF while retaining HF details
#
b_srffile = os.path.join(a_indir, "%s_dsmp5x5.srf" % (r_outroot))
 
srf_downsample_bin = os.path.join(install.A_GP_BIN_DIR, "srf_downsample")
progstring = ("%s ncoarsestk=5 ncoarsedip=5 < %s > %s 2>> %s\n" %
                      (srf_downsample_bin, a_srffile, b_srffile, self.log))
bband_utils.runprog(progstring)
 
# change a_srffile to name of downsampled file (so don't have coding below)
a_srffile = os.path.join(a_indir, "%s_dsmp5x5.srf" % (r_outroot))
#
# end downsample
#
 
The attached “genslip_downsample.py.txt” has this new coding as well, for reference. I’m very much a novice at python so I wouldn’t be surprised if there are errors in what I’ve added. The intent of the above code is to downsample the SRF by a factor of 5 in both along strike and down-dip directions, while still retaining the detail of the rupture timing and slip distribution across the smaller subfaults. I’ve hardwired this for now (ncoarsestk=5 ncoarsedip=5), but obviously these parameters could be exposed later. I also renamed the SRF to reflect the changes. Hopefully none of this breaks anything.
 
When you run the BBP, you should set DLEN = 0.02 and DWID = 0.02 (20 meters for each). When the downsampling is done, the resulting SRF will have 100 m subfaults.
