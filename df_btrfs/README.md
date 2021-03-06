# df_btrfs
This check monitors special cases of btrfs when it comes to data and metadata
allocation. Misbalanced Metadata can lead to - No space left on device - errors,
that prevents writing new data even if there is enough space left according to **df**
output. A very in depth explanation of the problem can be found here:
**http://marc.merlins.org/perso/btrfs/post_2014-05-04_Fixing-Btrfs-Filesystem-Full-Problems.html** .
Currently there is only one additional logic implemented in addition to **df** check that 
test for misbalanced metadata in the following way:

If at least one device in the **btrfs fi show** output is fully utilised like this:

   -> ...

   -> devid    1 size **133.04GB** used **133.04GB** path /dev/sdf

   -> ...

 and more then **73%** respectively **75%** of the Metadata space in **btrfs fi df /mount/point** 
 output:

   -> ...

   -> System: total=4.00MB, used=0.00

   -> Metadata, DUP: total=**3.00GB**, used=**2.32GB**      # ~77.33%

   -> ...

 is used, a warn respectively critical state is raised. Threshold is taken from btrfs wiki, 
 it can't be configured. See this link for more details:

 **https://btrfs.wiki.kernel.org/index.php/Problem_FAQ#I_get_.22No_space_left_on_device.22_errors.2C_but_df_says_I.27ve_got_lots_of_space**


# Installation
* $ su - SITENAME
* OMD[SITENAME]:~$ wget 'https://github.com/seppovic/check_mk-plugins/raw/master/df_btrfs/df_btrfs-1.0.7.mkp'
* OMD[SITENAME]:~$ check_mk -P install df_btrfs-1.0.7.mkp

# Requirements
* A check_mk_agent patched with this patch:
 https://raw.githubusercontent.com/seppovic/check_mk-plugins/master/df_btrfs/agents/check_mk_agent.linux.btrfs.patch
* A btrfs filesystem, obviously.

# TODO
* The current check does not catch all situations where btrfs stops can't write to disk. Further investigation necessary.

# Known issues

# History
* 1.0.7 Fixed few bugs, added metadata graph.
* 1.0.3 Fixed Bug in WATO, not using named Parameters anymore, makes this plugin usable with check_mk pre 1.2.7
* 1.0.2 Fixed Bug initialised variable to avoid Errors and fixed root reserve calculation
* 1.0.1 Fixed Bug in conversion function
* 1.0   First release

