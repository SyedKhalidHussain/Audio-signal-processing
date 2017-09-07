# Audio-signal-processing
# Some manipulation of content of .wav file and then plot for comparsion including spectrogram


import urllib.request
import matplotlib.pyplot as plt
import wave
import scipy.io.wavfile
import numpy as np
import pylab as pl

fhandle = urllib.request.urlopen('http://www.nch.com.au/acm/11k16bitpcm.wav')           # get the file handle of url

file_name = 'english.wav'                               # craete new .wav file
fh = open(file_name,'wb+')                              # get the file handle of a file "english.wav" using open command in write and append mode
fh.write(fhandle.read())                                # write the content of fhandle on "file_name" using fh.write
fh.close()                                              # close the file "fh"

wavefile = wave.open(file_name,'r')                                        # get the file handle of .wav file using wave.open command in read mode
parameter = wavefile.getparams()                                           # get the parameters from the wave file handle "wavefile"
nchannels, sample_width, framerate, numframes = parameter[:4]              # assign the first four parameters
sample_rate, data = scipy.io.wavfile.read(file_name)                       # get the sample rate and data of file "english.wav"
#new = wavefile.readframes(numframes)                                      # alternative way to get the data using "readframes" method
#newdata = np.fromstring(new, dtype = np.int16)                            # assign the data type to "numpy.int16"

newdata = data*0.2                                                         # decrease the signal content
newdata = newdata.astype(np.int16)                                         # assign the data type to "np.int16"
scipy.io.wavfile.write('modified.wav', sample_rate, newdata)                     # store the new data (keep the dtype unchaged i.e. int16) with the same sampling rate into "modified.wav"

plt.figure(1)
plt.subplot(2,1,1)
plt.title('Original')
plt.plot(data)
plt.subplot(2,1,2)
plt.title('Modified')
plt.plot(newdata)

pl.figure(2)
result = pl.specgram(newdata, NFFT = 1024, Fs = sample_rate, noverlap=900)                  # visualize the spectrogram of signal "newdata" using FFT of data blocks
pl.show()
