# CS598_DLH - Recreation of "Implementation for Improving Clinical Outcome Predictions Using Convolution over Medical Entities with Multimodal Learning"

Reference to Original Paper - [Link](https://arxiv.org/abs/2011.12349)

Reference to Original Code - [Link](https://github.com/tanlab/ConvolutionMedicalNer)

This code was cloned from Original Code, but we modified sections of code to conduct experiments on Google Colab. 
All experiments were performed on google colab plus.

## Steps to recreate

1. MIMIC-III dataset is available using the below link

    ```
    MIMIC-III dataset https://mimic.physionet.org/

    ```

2. MIMIC-III extraction pipeline - MIMIC-III data can either be extracted by setting up local instance of database or by using pre-processed file. 
   1. Setup Local instance and extract
      - To install MIMIC-III database locally use this [Link](https://mimic.mit.edu/docs/gettingstarted/local/install-mimic-locally-ubuntu/)
      - Then extract MIMIC-III data using [Link](https://github.com/MLforHealth/MIMIC_Extract)
   2. A pre-processed version of MIMIC-III  in [Link](https://github.com/MLforHealth/MIMIC_Extract)

    Both these approaches produce file "all_hourly_data.h5" which will be used for further processing.

3. Clone code in the below link and  change directory to `cs598_dlh_paper211`
   
    ```
    https://github.com/prashgs-uiuc/cs598_dlh_paper211

    ```

4. Place `all_hourly_data.h5` from MIMIC-III extract to `data` folder.

5. Copy other MIMIC-III data files `ADMISSIONS.csv`, `NOTEEVENTS.csv`, `ICUSTAYS.csv` files into `data` folder. 

6. Copy ClinicalBERT Gensim models to `Word2Vec` and `FastText` models to `embedding` folder.

    ```
    https://github.com/kexinhuang12345/clinicalBERT

    ```

7. Upload cs598_dlh_paper211 folder and its content to Google Drive. Access notebooks in a sequence using Google Colab.

8. Steps 9-20 requires Google Colab Pro version. Using Free version of Google Colab will increase training time and in some instances can lead to failure because of long run times.

9.  Open `01-Extract-Timseries-Features.ipnyb` notebook from Google drive. This opens up an instance of Google Colab.
   Select the Runtime as Python3 with GPU support  and High-Ram support. This notebook extracts `patients` and `vital labs` information. This notebook extracts LOS > 2,4 and 7 in addition to 3 and 5 which are mentioned in the Original paper 

10. To run every notebook in Google colab, Google drive has to be mounted. Each notebook contains instruction to mount or force remount Google drive.

11. This link provides additional information about mounting drive in Google Golab  

    ```
    https://medium.com/ml-book/simplest-way-to-open-files-from-google-drive-in-google-colab-fae14810674

    ```   

12. Open and Run `02-Select-SubClinicalNotes.ipynb` which selects sub clinical notes from various categories like Nursing, Radiology etc.

13. Running `03-Prprocess-Clinical-Notes.ipnyb` produces pre-processed clinical notes. Note, that this notebook uses `preprocess.py` module that is present in the base folder. Reference to preprocessing script is mentioned below.
    
    ```
    Preprocessing Script: https://github.com/kaggarwal/ClinicalNotesICU

    ```

14. Open `04-Apply-med7-on-Clinical-Notes.ipynb` to extract medical entities. Med7 is used to extract Clinical NERs.

    ```
    med7 implementation: https://github.com/kormilitzin/med7
    ```

    Run the following command install Med7 language model and load it using spacy. After the installation complete, comment this line, and make sure to restrart Google Colab runtime. This notebook takes about 16 hours to complete.  

    ```
    ! pip install https://huggingface.co/kormilitzin/en_core_med7_lg/resolve/main/en_core_med7_lg-any-py3-none-any.whl

    ```

15. Open and run `05-Represent-Entities-With-Different-Embeddings.ipynb` which will convert medical entities recognized in clinical notes to word embeddings for Word2Vec, FastText and a Combined embedding.

16. Open and run `06-Create-Timeseries-Data.ipynb` to create timeseries data for use in GRU model

17. Open `07-Timeseries-Baseline.ipynb`, run the below pip install to force installation of `h5py v2.10`.   
    After the installation is complete, comment and restart and run the notebook again. Note, that this notebook will be treated as tensorflow v1.x script even though newer version of tensorflow is availanble on google colab. This can be done by placing `%tensorflow_version 1.x` at the start of the script. This notebook produces Timeseries baseline model that uses GRU network.

    ```
    ! pip install 'h5py==2.10.0' --force-reinstall
    ```

18. Open `08-Multimodal-Baseline.ipynb` and run pip install to force installation of `h5py v2.10`. Follow the same steps as Step 16. Running this notebook produces `Multimodal Avg model` which is the second baseline model.

19. Open `09-Proposed-Model.ipynb` and run pip install to force installation of `h5py v2.10`. This model produces `proposed multimodal convolution model`. To perform experiments hyperparameters in this notebook can be changed. New `SEED` value can be used if you don't want the results to be overwritten. 

20. Open and run `10-Proposed-Model.ipynb` with appropriate path to see results from all the above models. 

Note: This repository also includes `09-Proposed-Model-pytorch.ipynb` which is the work we started to reimplement proposed model in pytorch but we couldn't complete it.

