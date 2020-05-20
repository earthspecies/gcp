# Notes on librosa

I've bumped into this a couple of times ... on GCP and Binder if I remember
correctly. ```libsndfile``` is a GNU licensed C library for [reading and writing sound files containing sampled audio data](https://github.com/erikd/libsndfile).

```
---------------------------------------------------------------------------
OSError                                   Traceback (most recent call last)
<ipython-input-13-caf53d36bd85> in <module>
----> 1 import librosa # the librosa library makes it very convenient to work with audio files in Python

/opt/conda/lib/python3.7/site-packages/librosa/__init__.py in <module>
     10 # And all the librosa sub-modules
     11 from ._cache import cache
---> 12 from . import core
     13 from . import beat
     14 from . import decompose

/opt/conda/lib/python3.7/site-packages/librosa/core/__init__.py in <module>
    124 
    125 from .time_frequency import *  # pylint: disable=wildcard-import
--> 126 from .audio import *  # pylint: disable=wildcard-import
    127 from .spectrum import *  # pylint: disable=wildcard-import
    128 from .pitch import *  # pylint: disable=wildcard-import

/opt/conda/lib/python3.7/site-packages/librosa/core/audio.py in <module>
      8 import warnings
      9 
---> 10 import soundfile as sf
     11 import audioread
     12 import numpy as np

/opt/conda/lib/python3.7/site-packages/soundfile.py in <module>
    140     _libname = _find_library('sndfile')
    141     if _libname is None:
--> 142         raise OSError('sndfile library not found')
    143     _snd = _ffi.dlopen(_libname)
    144 except OSError:

OSError: sndfile library not found
```

There are two solutions.

## 1. Conda

Conda behaves nicely and installs libsndfile as a dependency.

```
jupyter@esp:~$ conda install -c conda-forge librosa

The following NEW packages will be INSTALLED:

  audioread          conda-forge/linux-64::audioread-2.1.8-py37hc8dfbb8_2
  libflac            conda-forge/linux-64::libflac-1.3.3-he1b5a44_0
  libogg             conda-forge/linux-64::libogg-1.3.2-h516909a_1002
  librosa            conda-forge/noarch::librosa-0.7.2-py_1
  libsndfile         conda-forge/linux-64::libsndfile-1.0.28-he1b5a44_1000
  libvorbis          conda-forge/linux-64::libvorbis-1.3.6-he1b5a44_2
  pysoundfile        conda-forge/noarch::pysoundfile-0.10.2-py_1001
  resampy            conda-forge/noarch::resampy-0.2.2-py_0
  ```

## 2. pip

Of note, it's interesting that the default python environment for VMs on GCP is
now conda. This means that the above method is likely the best.

```
import sys
sys.executable

'/opt/conda/bin/python'
```

If we do install with pip, we have to note that ```pip install librosa``` installs the [SoundFile](https://pypi.org/project/SoundFile/) library, but not the C library it depends on. (🤦 Is this a constraint within pip or a bug in the package?)

```
sudo apt-get install libsndfile1-dev

Setting up libvorbis0a:amd64 (1.3.5-4+deb9u2) ...
Setting up libogg-dev:amd64 (1.3.2-1) ...
Setting up libvorbisfile3:amd64 (1.3.5-4+deb9u2) ...
Setting up libflac8:amd64 (1.3.2-1) ...
Setting up libvorbisenc2:amd64 (1.3.5-4+deb9u2) ...
Setting up libvorbis-dev:amd64 (1.3.5-4+deb9u2) ...
Setting up libflac-dev:amd64 (1.3.2-1) ...
Setting up libsndfile1:amd64 (1.0.27-3) ...
Setting up libsndfile1-dev (1.0.27-3) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
```
