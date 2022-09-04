# LIT-PCBA-parser
A parser written in rust for accessing the data of https://pubmed.ncbi.nlm.nih.gov/34885952/. Data available at http://bits.csb.pitt.edu/files/
gninavs, but a smaller (250MB) subset is included in this repo for demonstration purposes

This is the first bigger project I've built in Rust. Notable features includes multiprocessing with two seperate worker pools.

The objective of the parser is to extract the molecular structures from the zipped files like example_data/input/TP53/AID651631_inactive_0-lig_2vuk-rec_docked.sdf.gz.
The challenge is that these structures are split over many of these zipped files, and each molecule has upwards of 120 3D structures spread across several of these files, 
but we are only interested in the best scoring 3D structure per molecule. The scores of these 3D structures are available in the file example_data/input/TP53/newdefault.summary.gz.
The total dataset contains 15 targets for which this needs to be accomplished, however only one target is included here, as it alone is 250 MB.

From a programming perspective, the most notable feature of the project is a two-pool multiprocessing implementation, where one pool is responsible for iterating through
the "newdefault.summary.gz" of each target and figuring out which 3D structure to extract from the AID651631_(in)active_*-lig_2vuk-rec_docked.sdf.gz files. Once a sufficient chunk
has been extracted, several jobs are sent to a pool of decompression workers, which unzip the appropriate files and extract the appropriate 3D structures and return the result
to the calling worker.

Other utility functions which I'm using in the project https://github.com/amfaber/POR-DD have also been included as subcommands in the fully fledged CLI tool.
