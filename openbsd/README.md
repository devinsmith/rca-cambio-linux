# OpenBSD on the RCA Cambio 10.1 Tablet

# Intro

I do not currently use OpenBSD on the RCA Cambio. There are a couple of show
stoppers that are important enough to me that warrent this decision. I do
occasionally retest new releases to see if these issues are resolved. The
dmesgs for recent releases are provided in this directory.

# Issues

- No support for the backlight. This likely requires an updated
intel\_dsi\_vbt.c file. [see
here](https://github.com/openbsd/src/blob/master/sys/dev/pci/drm/i915/display/intel_dsi_vbt.c#L662-L675)


