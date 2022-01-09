# A Script to Explore Connectivity between Different Brain Regions using data and code from the Allen Mouse Connectivity Brain Atlas

This script has been written using the Python programming language in Jupyter notebook<sup>1</sup>. Its purpose is to compare the measured projection densities of functionally-related regions in the mouse brain in order to investigate their connectivity. The original experiments used to produce this data are sourced from the Allen Mouse Brain Connectivity Atlas <sup>2</sup>. These experiments initially comprised injections of an eGFP-AAV tracer into various regions of the mouse brain <sup>2</sup>. Fluorescence elicited by the tracer in all neurons connected to the site of injection was subsequently imaged and measured <sup>2</sup>. Projection density describes the number of pixels in a given structure eliciting fluorescence, or a 'projection', relative to the total number of pixels in that structure. Much of this script has been written using code from the Mouse Connectivity Jupyter Notebook<sup>3</sup>, which is freely available on the Allen Software Development Kit website to assist users in downloading and manipulating the data (https://allensdk.readthedocs.io/en/latest/_static/examples/nb/mouse_connectivity.html).

MouseConnectivityCache is a class that enables the user to download information from all experiments responsible for producing the data available in the Allen Mouse Brain Connectivity Atlas (without needing to specify the working directory). Therefore, sample data has not been explicitly provided alongside this script and README file. Examples of measurements taken in these experiments include projection density, projection volume (area in mm<sup>3</sup> in which projection signal is measured per structure) and sum_pixel_intensity (combined intensity of all pixels in a given structure). A complete list of all experimental variables and what they represent is available at https://allensdk.readthedocs.io/en/latest/unionizes.html.

The first lines of code in the script (taken from the Mouse Connectivity Jupyter Notebook<sup>3</sup>) should import data from the Allen Mouse Brain Connectivity Atlas<sup>2</sup> directly into your workspace:

>from allensdk.core.mouse_connectivity_cache import MouseConnectivityCache
>mcc = MouseConnectivityCache(manifest_file='connectivity/mouse_connectivity_manifest.json')
>all_experiments = mcc.get_experiments(dataframe=True)

This script can be edited to examine connectivity between different brain regions of interest. This would first require specifying which experiments you would like to analyse, which would depend on the site of eGFP-AAV tracer injection. This tracer produces fluorescence in neurons connected to the site of injection. The structures in which these neurons are present in can then be identified. Here, experiments in which injections were made into the lateral entorhinal cortex or Field CA3 were respectively used for analysis. One example of code in cell [4] which can be edited to specify the injected region would be:

> entl = structure_tree.get_structures_by_acronym(['ENTl'])[0] 
> entl_experiments = mcc.get_experiments(cre=False, 
                                        injection_structure_ids=[entl['id']])

Here, 'entl' refers to the lateral entorhinal cortex. An acronym is needed to specify the subregion of interest, which can be found in the original dataframe using (in cell [2]):

>structure_of_interest = structure_tree.get_structures_by_name(['region 1'])
>pd.DataFrame(structure)

Since analysing the subsequent projection densities yielded in all 316 subregions per hemisphere of the brain would be excessive, it is important to specify which structures you are interested in. Ideally, these structures would somehow relate to the site of injection, as you are measuring how well these structures physically connect to one another in the form of projection density. These structures could be related structurally or functionally. For example, if you were interested in subregions involved in producing and processing sense of smell, you could exclusively focus on projection densities yielded in olfactory structures. Examples of lines of code in cell [4] that can be edited to substitute-in different subregions:

> perirhinal_area = structure_tree.get_structures_by_acronym(['PERI'])[0]
> field_ca1 = structure_tree.get_structures_by_acronym(['CA1'])[0]
> field_ca2 = structure_tree.get_structures_by_acronym(['CA2'])[0]
> field_ca3 = structure_tree.get_structures_by_acronym(['CA3'])[0]
> dentate_gyrus = structure_tree.get_structures_by_acronym(['DG'])[0]
> subiculum = structure_tree.get_structures_by_acronym(['SUB'])[0]

> def region (column):
   if column['name'] == "Dentate gyrus" :
      return 'DG'
   if column['name'] == "Dentate gyrus, granule cell layer" :
      return 'DG'
   if column['name'] == "Dentate gyrus, molecular layer" :
      return 'DG'
   if column['name'] == "Dentate gyrus, polymorph layer" :
      return 'DG'

One flaw associated with the way in which this script was written is that it is assumes that you already know the subregions present in your regions of interest. In order to identify subregions present in the dataset, you can use code present in the Mouse Connectivity jupyter notebook<sup>3</sup>- specifically in cells [3] and [4]. This should give you a dataframe called summary_structures, which summarises all subregions present in the data. You can then search for subregions of interest in the 'name' column, which should retain the title of the overarching region with the subregion name appended to the end. 

There are a variety of additional ways in which the Allen Mouse Brain Connectivity Atlas data can be manipulated that have not been used here. More details can be found on the AllenSDK website (https://allensdk.readthedocs.io/en/latest/connectivity.html).

## References
<sup>1</sup> Kluyver T, Ragan-Kelley B, Pérez F, Granger B, Bussonnier M, Frederic J, et al. Jupyter Notebooks – a publishing format for reproducible computational workflows. In: Loizides F, Scmidt B, editors. Positioning and Power in Academic Publishing: Players, Agents and Agendas. IOS Press; 2016. p. 87–90.
<sup>2</sup> Oh SW, Harris JA, Ng L, Winslow B, Cain N, Mihalas S, et al. A mesoscale connectome of the mouse brain. Nature. 2014 Apr 10;508(7495):207–14.
<sup>3</sup> AllenSDK: code for reading and processing Allen Institute for Brain Science data [Internet]. Github; [cited 2022 Jan 9]. Available from: https://github.com/AllenInstitute/AllenSDK