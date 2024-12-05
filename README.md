# word2vec_antipsychotics
An accessible, timely method for repurposing medications: hypothesis generating; testing and validation. 

Unsupervised machine learning can uncover hidden patterns and make novel discoveries. We present a method to propose candidate medications for repurposing, as well as test and validate their utility. 

1) Hypothesis Generation
1.1) Download Pubmed Abstracts
The Word2Vec algorithm can be trained on large quantities of PubMed abstracts to identify medication to repurpose. To make our methods reproducible and easy to access, we ran our computing in Google Colab. Ideally, when having access to high perfromance computing with RAM of 360GB or more, all abstracts can be used, instead of just a chunk.

The National Library of Medicine has made the whole of PubMed downloadable free of charge at https://ftp.ncbi.nlm.nih.gov/pubmed/baseline/. In its entirety, the file would be large (~360GB), making it cumbersome to handle and hard to use in its entirety. Hence, we train our model on a moderate chunk of it:  ~2,5 million abstracts out of ~30 million, from the years 2000 - December 2023. 

The original repository includes 1219 individual files. There appears to be no clear logic behind the labelling of the files by the repository creators. After inspection of some of the files, they were sorted somewhat chronological. Still, there was a large degree of overlap; later files e.g. file no. 1000 still contains abstracts from the 1960, while file no. 500 contains studies from the 2000s.  We were able to download the whole of pubmed abstracts using the code labelled 'Download PubMed Repository', but only able to merge the last 90 files (1129-1219). We chose these as they appear to contain mostly the newest abstracts on PubMed. This was done using code also contained in 'Download PubMed Repository'.


1.2) Train Word2Vec
We used a modified Word2vec implementation in gensim (https://radimrehurek.com/gensim/) ran on Google Colab to generate the word vector representations trained on ~2,5 million abstracts from PubMed between the years 2000-2023. Hyperparameters were: 300-dimensional embeddings, learning rate = 0.025 decreasing to 0.0007, context window of 3, minimum count of 2, workers 31, negative 5, sample 6e-5 with a Continuous Bag of Words model instead of skip-gram. These were found through optimisation for performance on ground truths. 


1.3) Medication Screening
We screened clozapine as it is the only compound reserved for patients with treatment-resistant schizophrenia due to its potentially hazardous side effects profile.  

A new prediction was every word that was close to the input word, was a medication and had not been mentioned together with the following term in a PubMed abstract: (schizo* OR psychot* OR psychosis OR antipsychotics OR Clozapine) between 2000-2023. In cases where there were five or less abstracts were a word and the above query were mentioned together, we checked if the abstracts in question were featured in our training corpus. This was always a real possibility, since, as described earlier, only a fraction of all PubMed abstracts were included as source data. If the abstracts were not featured in the training corpus, the word was considered a prediction, since the model did not know about a connection of the two words through abstracts. 

When recreating this, any medication can be screened. For example, you could be searching for 'Levodopa' and see if there are words close to it that arent ususally associated with it. 

2) Hypothesis Testing
2.1) Methodology
The proposed medication related to an antipsychotic that was identified by our model were evaluated using the Medical Information Mart for Intensive Care database (MIMIV-IV; Johnson et al., 2023). MIMIC-IV is a publicly available database containing deidentified electronic health records from over 300,000 patients admitted to the Beth Israel Deaconess Medical Center (BIDMC) in Boston, MA, USA, between 2008-2022. To get access, apply at https://mimic.mit.edu/docs/gettingstarted/.

To test and validate the medications we used a prospective cohort design. Participants were identified as being prescribed the medication, compared to patients not receiving the medication at an index hospitalisation not related to the disease under investigation. They were then followed up to determine their time to first admission with the disease under investigation.

2.2) Choosing which newly suggested medication to investigate for protective properties
We searched in MIMIC-IV which of the top 5 newly suggested medications had the most patients with a target diagnosis and received the medication in the MIMIC dataset. 
Code for this can be found in 'Choosing which newly suggested medication to investigate for protective properties'.

2.3) Data extraction MIMIC
To test and validate the medications we used a prospective cohort design. Participants were identified as being prescribed the medication, compared to patients not receiving the medication at an index hospitalisation not related to the disease under investigation. They were then followed up to determine their time to first admission with the disease under investigation.

Primary outcome: Time from discharge of index admission (unrelated to psychosis) to hospital admission with a psychosis. Patients were censored at their last known hospital record without a psychosis or related disorders.
Primary exposure: Use of the class of medication (proposed by our algorithm) at the index hospitalisation. 
Secondary exposure: Use of the specific medication (proposed by the algorithm) at the index hospitalisation.  


3. Hypothesis Validation

2.3) Data extraction CRIS
CRIS data was extracted by a member of the CRIS Team at SLaM Trust. 
No code can be given for this. 



















