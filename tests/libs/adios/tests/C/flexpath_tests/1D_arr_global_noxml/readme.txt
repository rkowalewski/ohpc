# The protocol file generated by protogen.py ver. 1.0 on 2013-06-27 12:54
# AUTHOR: Magda S aka Magic Magg magg dot gatech at gmail dot com
# DATE: 2013-06-27


DESCRIPTION
===========
This is a test case for ADIOS. Analogous to 1D_arr_global, but in the noxml version.

The idea of the test. The writer writes a
1D array of NX double elements. The array is written as ADIOS global array
so each process writes in a global space, but in its own place.

The writer writes the 1D array and the reader reads the 1D array of doubles.

There might be as many writers as you wish, and there might be as many readers
as you wish. However, each rank reads its own rank. The reader knows how many 
writers were there so if its rank is higher then it quits.

The test uses might be run in two modes (use '-t' option with the appropriate mode).

1. MPI/ADIOS_READ_METHOD_BP
2. FLEXPATH/ADIOS_READ_METHOD_FLEXPATH


BUILD
=======
# you need to set the environment variables as Makefile uses those locations 
# to locate libraries and headers

export ADIOS_ROOT=/rock/opt/adios/git-dbg
export MXML_ROOT=/rock/opt/mxml/2.7
export MPI_ROOT=/rock/opt/openmpi/1.6.3
export EVPATH_ROOT=/rock/opt/evpath

# in certain cases you might need the lustre directory (e.g., on kraken)
export LUSTRE_ROOT=/opt/cray/lustre-cray_ss_s/default

# build the MPI/ADIOS_READ_METHOD_BP
$ make -f Makefile.generic

# should remove all unnecessary exec files 
$ make -f Makefile.generic clean

# cleans files hanging around after previous runs
$ make -f Makefile.generic clean_test

RUN
===== 
# to clean files hanging around after previous runs
$ make -f Makefile.generic clean_test

$ mpirun -np 2 writer -t flx
$ mpirun -np 2 reader -t flx

See Makefile for other options of running the test or use '-h'.


OUTDATED (2013-07-08)
======================
I will update later. 

RUNNING ON KRAKEN
=================
This runs one reader per node and one writer per node.

aprun -n 1 -N 1 ./arrays_read &
aprun -n 1 -N 1 ./arrays_write

You should be able to run the example with as many readers and writers as you wish.

Example PBS script
------------------
#!/bash/bin
#PBS -l walltime=00:05:00,size=24
#PBS -A UT-TENN0033

date

echo "nodefile="
cat $PBS_NODEFILE
echo "=end nodefile"

# make sure you have all modules loaded
module use ~smagg/opt/modulefiles
module load mag-mxml-2.7/kraken-gnu
module list

cd $PBS_O_WORKDIR

aprun -n 1 -N 1 ./arrays_read &
aprun -n 1 -N 1 ./arrays_write

date

# eof


TROUBLESHOOTING
===============

There might be text files left; they should be removed for the next run.

# EOF
