# Fast data portals for scRNAseq data

This repository contains tools for making web portals and interactive tools used for exploring and visualising single cell RNA data.

## Interactive tools and web portals:

### web_portal.sh
* see an example [here](https://developmentcellatlas.ncl.ac.uk/datasets/liver_10x/)
* the pipeline can be called with _web\_portal.sh_
* this take as input an option file
* option files should be placed in the options folder
* inside this folder there are two examples: one option file for a seurat object (_liver\_web\_portal\_options.txt_) and on option file for scanpy objects (_thymus\_web\_portal\_options.txt_)
* the format of the option file:
     * line 1 indicates data file: "file\_name: ../../data/mydata.RDS"
     * line 2 indicates output folder: "output\_folder: output\_here/my\_dear\_web\_portal"
     * line 3 indicates what dimenssionality reduction types to be included: "dr\_coordonates: UMAP->X\_umap; tSNE->X\_tsne; FDG->X\_draw\_graph\_fa;". Each field must end with ";". The dr fields must be in the `@dr` slot for seurat objects or in `data.obsm[dr_coor]` attribute for scanpy objects. 
     * line 4 indicates meta data fields to include in the portal: "Cell Labels->Annotation->null;Flow gate->sort->null;Sample->fetus->null;Gender->gender->null;". Each field must end with ";". Each field indicate the name of the meta data column followed by "->" followed by name of the field to appear in the data portal (for example you might have a meta data column called "sort.ids" but on the web portal you want it to appear as "Sorting gates") followed by "->" followed by the name color key (csv file found in the resource folder) or set to "null" ir such a color key does not exist for a particular meta data and random colours must be generated instead.
* the output is tar.gz file. Unziping this will generate a folder with lots of contents (the web portal folder)
* if you require password protection go to the folder _templates\_password\_protection_ and run the Python script _generate\_password.py_. This will insert a new password to the template. The password is written to the file _password.txt_. Keep this for reference.
* copy paste to web portal folder the 4 files from the folder _templates\_password\_protection_ (if you need password protection) or from folder folder _templates\_no\_password\_protection_ (if you do not need password protection)
* these files are: _fetch\_category.php_, _fetch\_dr\_coordinates.php_, _fetch\_gene\_expression.php_, _index.php_
* upload to web server
  
### interactive_heatmap_dotplot.sh
* see an example [here](https://developmentcellatlas.ncl.ac.uk/datasets/fetal_liver_interactive_gene_expression_heatmap.html)
* this is used to generated an interactive heatmap/dot plot from a seurat object or scanpy object
* takes one argument which is the path of an option files
* there are two examples of option files in the folder _options_: _options\_fetal\_liver.txt_ for a seurat object and _options\_fetal\_thymus.txt_ for a scanpy object
* format of the option file:
    * line 1: data file path
    * line 2: meta data column by which to assign the identity of cells
    * line 3: name of output folder
    * line 4: name of interactive html page
    * line 5: a short line describing the data which will be included in the interactive page
* IMPORTANT NOTICE: if the vector partitioning the data (i.e. meta data column) is using integer indices (e.g. Louvain clustering which assigns integer identifies to clusters) it is highly recommended to pre-append the tag "Cluster\_" to all indices (e.g. "1" and "103" becomes "Cluster\_1" and "Cluster\_103" respectively). Failure to do so will not raise any errors, but the resulting interactive heatmap/dot plot will have glitches.
 
### pseudotime_webportal.sh
* see an example [here](https://developmentcellatlas.ncl.ac.uk/datasets/pseudotime_liver_blin/)
* this creates a web portal useful for exploring the results of a trajectory analysis
* this only works on seurat objects, and not on scanpy objects
    * _seurat.addr_ full or relative path of the file storing the seurat object or scanpy object
    * _set.ident_ meta data column used for partion the data
    * _cell.types_ an R character vector listing the cell types to used in the trajectory. White spaces in the names must be replaced with double at sign ("@@").
    * _root\_cell\_type_ name of the cell type to use as root
    * _type.to.colours_ full or relative path to the color key (a csv file mapping cell types to colours). To generate your own colour key use _color\_management.html_ (see [here](https://github.com/haniffalab/Single-cell-RNAseq-data-analysis-bundle)) 
    * _lineage.name_ lineage name that appears in the portal. White spaces must be replaced with double at sign ("@@" instead of " ")
 
### gene_grouping.sh
* creates a graphical interface for fast data exploration of gene expression. 
* the resulting program also groups genes by expression patterns and allows the user to manually change group assignment
    * _seurat.addr_ full or relative path of the file storing the seurat object
    * _no\_clusters_ number of gene clusters required
* the output is a folder
* copy the _gene\_viewer.py_ script to the outputted folder and then you can run the GUI from a python shell
   