# Shapes-Network-And-Verification-Ongoing-

The colab links have this:

from google.colab import drive
drive.mount('/content/drive')
fold = "drive/My Drive/Colab Notebooks/smtdatatrials2/trials2/" 
import sys
sys.path.insert(1, fold)
trials= fold+"sample"


This is used to specify paths. It gives the place where files will be saved and loaded from.


Running the line and rectangle file will create data for the two shapes.
For the third data generation file(which applies rotation and inversion), we saved a base file with some 
letters(which were not code generated), and ran the code which applied rotation and inversion to get a larger data set.
We kept the number of data points for each shape to be roughly the same, so of the lines and rectangles, a varied 1000 of each were
selected for the network. 
Note: In data creation, running the code multiple times will create multiple copies.

For the neural network code, we just loaded the data in and used tensorflow to create a network with a bit over 70% accuracy.

Then, saved this model into a file and loaded it into the z3 code. In this, there is a function with a parameter k whose purpose is to 
give a set of weights which you append to the network to end up with a new network which outputs 0 if the original ouput was the kth 
class, and greater than 0 otherwise.
The function id applies labels to a layer given the labes to the next layer. Then there is a function splitnetwork which creates the
labelled network based one increment/decrement and positive/negative labels on each node.
Merging is done using a partition(which is assumed to be consistent with the labelling). So, for each layer, a list of lists is present,
where each of the inner lists gives which all neurons of the original network(the one we get after applying the splitnetwork function)
are to be merged. For this as well, we first create a function which does this for a layer, then recursively do this on the network
using another function to do the abstraction.

Refining takes an abstracted network, the original network as well as the partitions involved(to see which all were merged), and
finds which neuron in the original created the biggest difference. This will then be removed from the partition it is in, and kept as
a partition by itself. Then abstraction is done using this new, refined list of partitions.

Cegar loop is as in the paper, and the completed property is to check if squares are classified as rectangles by the network. 
