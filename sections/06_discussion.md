## Discussion

*This section provides meta-commentary that spans the Categorize, Study, and
Treat subject areas.  The candidate sub-sections below are initial ideas that
can be further pruned.*

### Evaluation

*What are the challenges in evaluating deep learning models that are specific to
this domain?  This can include a discussion of ROC versus precision-recall
curves for the imbalanced classes often encountered in biomedical datasets.
It could also mention alternative metrics that are used in specific sub-areas
such as enrichment factors in virtual screening.  A lack of true gold standard
data for some problems complicates both training and evaluation.  Confidence-
weighted labels are valuable when available.*

*Is progress in some biomedical areas slowed when new predictions (e.g. from
generative models) cannot be assessed by any human expert and require
experimental testing?  For example, contrast a painting or song generated by
a GAN versus a novel chemical compound.  Related is the idea that on some tasks
(e.g. the recent wave of deep learning versus MD image classification papers)
it is easy to tell when deep learning has produced a breakthrough because
human-level performance is an impressive baseline.  In many tasks we reviewed,
human-level performance is irrelevant.*

### Interpretation

As the challenge of interpretability is common across many domains, there is
significant interest in developing generic procedures for knowledge extraction
from deep models. Ribeiro et al [@tag:Ribeiro2016_lime] focus on interpreting
individual predictions rather than interpreting the model. By fitting simple
linear models to mimic the predictions of the deep learning model in a small
neighborhood of a data sample, they generated an interpretable model for each
prediction. While this procedure can provide interpretable models for each
sample, it is unclear whether these interpretable models are reliable.
Theoretical guarantees on the curvature of the predictions of deep learning
models are not known, and it is unclear whether predictions from deep learning
models are robust to sample noise. Toward quantifying the uncertainty of
predictions, there has been a renewed interest in confidence intervals for
deep neural networks. Early work from Chryssolouris et al
[@tag:Chryssolouris1996_confidence] provided confidence intervals under the
 assumption of normally distributed error. However, Nguyen et al
 [@tag:Nguyen2014_adversarial] showed that the confidence of convolutional
 neural networks is not reliable; they can output confidence scores over
 99.99% even for samples that are purely noise. Recently, Fong and Vedaldi
 [@tag:Fong2017_perturb] provided a framework for understanding black box
 algorithms by perturbing input data.

For domain-specific models, we previously described approaches for the
interpretation and visualization of neural networks that prediction
transcription factor binding [@tag:Alipanahi2015_predicting
@tag:Lanchantin2016_motif @tag:Shrikumar2016_blackbox]. Other studies have
primarily focused on integrating attention mechanisms with the neural networks.
Attention mechanisms dynamically weight the importance the neural network gives
to each feature. By inspecting the attention weights for a particular sample, a
practitioner can identify the important features for a particular prediction.
Choi et al [@tag:Choi2016_retain] inverted the typical architecture of recurrent
neural networks to improve interpretability. In particular, they only used
recurrent connections in the attention generating procedure, leaving the hidden
state directly connected to the input variables. In the clinical domain, this
model was able to produce accurate diagnoses in which the contribution of
previous hospital visits could be directly interpreted. Choi et al
[@tag:Choi2016_gram] later extended this work to take into account the structure
of disease ontologies and found that the concepts represented by the model were
aligned with medical knowledge. Che et al [@tag:Che2015_distill] introduced a
knowledge-distillation approach which used gradient boosted trees to learn
interpretable healthcare features from trained deep models.

### Data limitations

*Related to evaluation, are there data quality issues in genomic, clinical, and
other data that make this domain particularly challenging?  Are these worse than
what is faced in other non-biomedical domains?*

*Many applications have used relatively small training datasets.  We might
discuss workarounds (e.g. semi-synthetic data, splitting instances, etc.) and
how this could impact future progress.  Might this be why some studies have
resorted to feature engineering instead of learning representations from low-
level features?  Is there still work to be done in finding the right low-level
features in some problems?*

###### Biomedical data is often "Wide"

*Biomedical studies typically deal with relatively small sample sizes but each
sample may have millions of measurements (genotypes and other omics data, lab
tests etc).*

*Classical machine learning recommendations were to have 10x samples per number
of parameters in the model.*

*Number of parameters in an MLP. Convolutions and similar strategies help but do
not solve*

*Bengio diet networks paper*


### Hardware limitations and scaling

*Several papers have stated that memory or other hardware limitations
artificially restricted the number of training instances, model inputs/outputs,
hidden layers, etc.  Is this a general problem worth discussing or will it be
solved naturally as hardware improves and/or groups move to distributed deep
learning frameworks?  Does hardware limit what types of problems are accessible
to the average computational group, and if so, will that limit future progress?
For instance, some hyperparameter search strategies are not feasible for a lab
with only a couple GPUs.*

*Some of this is also outlined in the Categorize section.  We can decide where
it best fits.*

Efficiently scaling deep learning is challenging, and there is a high
computational cost (e.g., time, memory, energy) associated with training neural
networks and using them for classification. As such, neural networks
have only recently found widespread use [@tag:Schmidhuber2014_dnn_overview].

Many have sought to curb the costs of deep learning, with methods ranging from
the very applied (e.g., reduced numerical precision [@tag:Gupta2015_prec
@tag:Bengio2015_prec @tag:Sa2015_buckwild @tag:Hubara2016_qnn]) to the exotic
and theoretic (e.g., training small networks to mimic large networks and
ensembles [@tag:Caruana2014_need @tag:Hinton2015_dark_knowledge]). The largest
gains in efficiency have come from computation with graphics processing units
(GPUs) [@tag:Raina2009_gpu @tag:Vanhoucke2011_cpu @tag:Seide2014_parallel
@tag:Hadjas2015_cc @tag:Edwards2015_growing_pains
@tag:Schmidhuber2014_dnn_overview], which excel at the matrix and vector
operations so central to deep learning. The massively parallel nature of GPUs
allows additional optimizations, such as accelerated mini-batch gradient
descent [@tag:Vanhoucke2011_cpu @tag:Seide2014_parallel @tag:Su2015_gpu
@tag:Li2014_minibatch]. However, GPUs also have a limited quantity of memory,
making it difficult to implement networks of significant size and
complexity on a single GPU or machine [@tag:Raina2009_gpu
@tag:Krizhevsky2013_nips_cnn]. This restriction has sometimes forced
computational biologists to use workarounds or limit the size of an analysis.
For example, Chen et al. [@tag:Chen2016_gene_expr] aimed to infer the
expression level of all genes with a single neural network, but due to
memory restrictions they randomly partitioned genes into two halves and
analyzed each separately. In other cases, researchers limited the size
of their neural network [@tag:Wang2016_protein_contact
@tag:Gomezb2016_automatic]. Some have also chosen to use slower
CPU implementations rather than sacrifice network size or performance
[@tag:Yasushi2016_cgbvs_dnn].

Steady improvements in GPU hardware may alleviate this issue somewhat, but it
is not clear whether they can occur quickly enough to keep up with the growing
amount of available biological data or increasing network sizes. Much has
been done to minimize the memory
requirements of neural networks [@tag:CudNN @tag:Caruana2014_need
@tag:Gupta2015_prec @tag:Bengio2015_prec @tag:Sa2015_buckwild
@tag:Chen2015_hashing @tag:Hubara2016_qnn], but there is also growing
interest in specialized hardware, such as field-programmable gate arrays
(FPGAs) [@tag:Edwards2015_growing_pains @tag:Lacey2016_dl_fpga] and
application-specific integrated circuits (ASICs). Specialized hardware promises
improvements in deep learning at reduced time, energy, and memory
[@tag:Edwards2015_growing_pains]. Logically, there is less software for highly
specialized hardware [@tag:Lacey2016_dl_fpga], and it could be a difficult
investment for those not solely interested in deep learning. However, it is
likely that such options will find increased support as they become a more
popular platform for deep learning and general computation.

Distributed computing is a general solution to intense computational
requirements, and has enabled many large-scale deep learning efforts. Early
approaches to distributed computation [@tag:Mapreduce @tag:Graphlab] were
not suitable for deep learning [@tag:Dean2012_nips_downpour],
but significant progress has been made. There
now exist a number of algorithms [@tag:Dean2012_nips_downpour @tag:Dogwild
@tag:Sa2015_buckwild], tools [@tag:Moritz2015_sparknet @tag:Meng2016_mllib
@tag:TensorFlow], and high-level libraries [@tag:Keras @tag:Elephas] for deep
learning in a distributed environment, and it is possible to train very complex
networks with limited infrastructure [@tag:Coates2013_cots_hpc]. Besides
handling very large networks, distributed or parallelized approaches offer
other advantages, such as improved ensembling [@tag:Sun2016_ensemble] or
accelerated hyperparameter optimization [@tag:Bergstra2011_hyper
@tag:Bergstra2012_random].

Cloud computing, which has already seen adoption in genomics
[@tag:Schatz2010_dna_cloud], could facilitate easier sharing of the large
datasets common to biology [@tag:Gerstein2016_scaling @tag:Stein2010_cloud],
and may be key to scaling deep learning. Cloud computing affords researchers
significant flexibility, and enables the use of specialized hardware (e.g.,
FPGAs, ASICs, GPUs) without significant investment. With such flexibility, it
could be easier to address the different challenges associated with the
multitudinous layers and architectures available
[@tag:Krizhevsky2014_weird_trick]. Though many are reluctant to store sensitive
data (e.g., patient electronic health records) in the cloud,
secure/regulation-compliant cloud services do exist [@tag:RAD2010_view_cc].

*TODO: Write the transition once more of the Discussion section has been
fleshed out.*

### Code, data, and model sharing

In addition to methodological improvements, a robust culture of data sharing -
and in particular the sharing of high-quality labeled datasets - would do much
to speed advances in this domain. The cultural barriers are perhaps best
captured by the implications of using the term "research parasite" to describe
scientists who use data from other researchers [@doi:10.1056/NEJMe1516564]. In
short, a field that honors only discoveries and not the hard work of generating
useful data will have difficulty encouraging scientists to share their hard-won
data. Unfortunately, it's precisely those data that would help to power deep
learning in the domain. Though not a methodological consideration, efforts are
underway to recognize those who promote an ecosystem of rigorous sharing and
analysis [@doi:10.1038/ng.3830].

*Reproducibiliy is important for science to progress. In the context of deep
learning applied to advance human healthcare, does reproducibility have
different requirements or alternative connotations? With vast hyperparameter
spaces, massively heterogeneous and noisy biological data sets, and black box
interpretability problems, how can we best ensure reproducible models? What
might a clinician, or policy maker, need to see in a deep model in order to
influence healthcare decisions? Or, is deep learning a hypothesis generation
machine that requires manual validation? DeepChem and DragoNN are worth
discussing here.*

### Multimodal, multi-task, and transfer learning

As discussed above, the fact that biomedical datasets often contain a limited
number of instances or labels can be a cause of poor performance of
machine learning algorithms. When trained on such datasets, deep learning
models are particularly prone to overfitting due to their high representational
power. However, transfer learning techniques also known as domain adaptation
enable transfer of extracted patterns between different datasets and even
domains. This approach can be described in a 2-step process:
(1) training a model for the base task, and (2) reusing the trained model
for the target problem in hand. The first step allows a model to take advantage
of larger amount of data, labels, or both, to extract better feature
representations. Transferring learnt features in deep neural networks
improves performance compared to randomly initialized features even
when pre-training and target sets are dissimilar. However,
transferability of features decreases as the distance between the base task
and target task increases [@tag:Yosinski2014].

In image analysis, previous examples of deep transfer learning applications
proved large scale natural image sets [@tag:Russakovsky2015_imagenet]
to be useful for pre-training models that can then serve as generic feature
extractors applied to various types of biological images
[@tag:Zhang2015_multitask_tl @tag:Zeng2015 @tag:Angermueller2016_dl_review
@tag:Pawlowski2016]. More recently, deep learning models trained to predict
protein sub-cellular localization successfully performed predictions for
proteins not originally present in the training set [@tag:Parnamaa2017].
Moreover, in this type of task, learnt features performed reasonably well even
when applied to images obtained using different fluorescent labels, imaging
techniques, and different cell types [@tag:Kraus2017_deeploc]. However, there
are no established theoretical guarantees for feature transferability between
distant domains such as natural images and various modalities of biological
imaging. Because learnt patterns are represented in deep neural networks in a
layer-wise hierarchical fashion, this issue is usually addressed by fixing an
empirically chosen number of layers that preserve generic characteristics
of both training and target datasets. Then, the model is fine-tuned by
re-training multiple networks' top layers on the specific dataset in order to
re-learn domain-specific high level concepts. For example, see fine-tuning for
radiology image classification [@tag:Rajkomar2017_radiographs].
Fine-tuning on specific biological datasets enables more focused predictions.
The Basset package [@tag:Kelley2016_basset] for prediction of functional
activities from DNA sequences was shown to rapidly learn and accurately predict
on new data by leveraging a model pre-trained on available public data.
To simulate this scenario, authors put aside 15 of 164 cell type datasets
and trained the Basset model on the remaining 149 datasets. Then, they
fine-tuned the model with one training pass of each of the remaining datasets
and achieved results close to the model trained on all 164 datasets together.
In another example, Min et al.
[@tag:Min2016_deepenhancer] demonstrated how training on the experimentally
validated FANTOM5 permissive enhancer dataset followed by fine-tuning on ENCODE
enhancer datasets improved cell type-specific predictions, outperforming
state-of-the-art results.

Multimodal learning is related to transfer learning. It assumes
simultaneous learning from various types of inputs, such as images and text.
It allows capture of features that describe common concepts across input
modalities. Generative graphical models like restricted Boltzmann
machines (RBM) and their stacked versions, deep Boltzmann machines (DBM), and
deep belief networks (DBN), demonstrate successful extraction of
more informative features for one modality
(images or video) when jointly learnt with other modalities
(audio or text) [@tag:Ngiam2011].
Deep graphical models such as DBNs are considered to be well suited for
multimodal learning tasks since they learn a joint probability
distribution from inputs. They can be pre-trained in an unsupervised fashion
on large unlabeled data and then fine-tuned on a smaller number of labeled
examples. When labels are available, convolutional neural networks (CNN) are
ubiquitously used since they can be trained end-to-end with backpropagation
and demonstrate state-of-the-art performance in many discriminative tasks
[@tag:Angermueller2016_dl_review].
Jha et al. [@tag:Jha2017_integrative_models] showed that an integrated training
approach delivers better performance compared to individual networks. They
compared a number of feed-forward architectures trained on RNA-seq data
with and without an additional set of CLIP-seq, knockdown, and over-expression
based input features. Results showed that the integrative deep model
generalized well for combined data, offering large performance improvement
for alternative splicing event estimation.
Chaudhary et al. [@tag:Chaudhary2017_multiom_liver_cancer] trained a deep
autoencoder model jointly on RNA-seq, miRNA-seq, and methylation data from TCGA
to predict survival subgroups of hepatocellular carcinoma patients. This
multimodal approach that treated different omics as different modalities
outperformed both traditional methods (PCA) and single-omic models.
Interestingly, multi-omic model performance did not improve when combined
with clinical information, suggesting that the model was able to capture
redundant contributions of clinical features through their correlated
genomic features.
Chen et al. [@tag:Chen2015_trans_species] used deep belief networks to learn
phosphorylation states of a common set of signaling proteins in primary
cultured bronchial cells collected from rats and humans treated
with distinct stimuli. By interpreting species as different modalities
representing similar high-level concepts, they showed that DBNs were able
to capture cross-species representation of signaling mechanisms in response
to a common stimuli. Another application used DBNs for joint unsupervised
feature learning from cancer datasets containing gene expression,
DNA methylation, and miRNA expression data [@tag:Liang2015_exprs_cancer].
This approach allowed for the capture of intrinsic relationships in
different modalities and for better clustering performance over conventional
k-means based methods.
Multimodal learning with CNNs is usually implemented as a collection of
individual networks in which each learns representations from single data type.
These individual representations are further concatenated before or within
fully-connected layers. FIDDLE [@tag:Eser2016_fiddle] is an example of
multimodal CNNs that represents an ensemble of individual networks that take
as inputs a number of genomic datasets, including NET-seq, MNase-seq, ChIP-seq,
RNA-seq, and raw DNA sequence to predict Transcription Start Site-seq
(TSS-seq) outputs. The combined model radically improves performance over
separately trained datatype-specific networks, suggesting that it learns the
synergistic relationship between datasets.

Multi-task learning (MTL) is an approach related to transfer learning.
In an MTL framework a model co-learns a number of tasks simultaneously such that
features are shared across them.
DeepSEA framework [@tag:Zhou2015_deep_sea] implemented a multi-task joint
learning of diverse chromatin factors sharing predictive features from raw
DNA sequence. This allowed, for example, a sequence feature that is effective
in recognizing binding of a specific TF to be simultaneously used by
another predictor for a physically interacting TF.
Similarly, TFImpute [@tag:Qin2017_onehot], a CNN-RNN architecture learned
information shared across transcription factors and cell lines to predict
cell-specific TF binding for TF-cell line combinations based on only a
small fraction (4%) of the combinations using available ChIP-seq data. On
multiple test sets that excluded specific TFs and cell lines, TFImpute showed
comparable or superior performance compared to the state-of-the-art.
Yoon et al.[@tag:Yoon2016_cancer_reports], previously discussed in the section
on Electronic Health Records, demonstrated that predicting the primary cancer
site from the cancer pathology reports together with its laterality
substantially improved the performance for the latter task, suggesting that
MTL can effectively leverage the commonality between two tasks using
a shared representation.
A number of studies previously mentioned in the section on developing new
treatments employed multi-task learning approach to predict a large number
of compound and target interactions for drug discovery
[@tag:Dahl2014_multi_qsar @tag:Ramsundar2015_multitask_drug]
and drug toxicity prediction
[@tag:Mayr2016_deep_tox @tag:Hughes2016_macromol_react]. Kearnes et al.
[@tag:Kearnes2016_admet] did a systematic comparison of single-task and
multi-task deep models on a set of industrial ADMET datasets. They confirmed
that multi-task learning can improve performance over single-task models. They
further showed that smaller datasets tend to benefit more from multitask
learning than larger datasets. Results emphasized that multitask effects are
highly dataset-dependent, suggesting the use of dataset-specific models to
maximize overall performance.

MTL approach is complementary to multimodal and transfer learning. All three
techniques can be used together in the same model. For example, Zhang et al.
[@tag:Zhang2015_multitask_tl] combined deep model-based transfer and multi-task
learning for cross-domain image annotation. One could imagine extending that
approach to multimodal inputs as well. Common characteristic of these methods
lies in better generalization of extracted features by leveraging relationships
between information in provided in inputs and task objectives, represented at
various hierarchical levels of abstraction in a deep learning model structure.

Despite demonstrated improvements, transfer learning approaches also pose
a number of challenges. As mentioned above, there are no theoretically sound
principles for pre-training and fine-tuning. Most best practice recommendations
are heuristic and have to take into account additional hyper-parameters that
depend on specific deep architectures, sizes of pre-training and target
datasets, and similarity of domains. However, similarity of datasets and domains
in transfer learning and relatedness of tasks in MTL are difficult to access.
Most current studies address these limitations by empirical evaluation of the
model using established best practices or heuristics and cross-validation.
Unfortunately, negative results are typically left out and not
presented in the final study publications.
Results by Rajkomar et al. [@tag:Rajkomar2017_radiographs] showed that a deep
CNN trained on natural images can boost radiology image classification
performance. However, due to differences in imaging domains, target task
required either re-training the initial model from scratch with special
pre-processing or fine-tuning of the whole network on radiographs with heavy
data augmentation to avoid overfitting. Exclusively fine-tuning top layers led
to much lower validation accuracy (81.4 vs 99.5).
Fine-tuning procedure for the discussed Basset model pre-trained on data
from different cell types required no more than one training pass. Otherwise,
the model started overfitting new data [@tag:Kelley2016_basset].
DeepChem successfully improved results for low-data drug discovery
with one-shot learning for related tasks. However, it demonstrated clear
limitations to cross-task generalization across unrelated tasks in one-shot
models, specifically nuclear receptor assays and patient adverse reactions
[@tag:AltaeTran2016_one_shot].

Overall, multimodal, multi-task and transfer learning strategies demonstrate
high potential for many biomedical applications that are otherwise limited
by data volume and presence of labels. However, these methods not only inherit
most methodological issues from natural image, text, and audio domains, but also
pose new challenges, specific to biological data. Making negative results,
source code, and pre-trained models publicly available helps to accelerate
progress in this direction. However, there are privacy considerations for models
trained on sensitive data, such as patient-related information (see Data
sharing section). Thus, there is a compelling need for the development of
privacy-preserving transfer learning algorithms, such as Private Aggregation
of Teacher Ensembles (PATE-G) [@tag:Papernot2017_pate], that can leverage
publicly available data. We suggest that these types of models deserve deeper
investigation to establish sound theoretical guarantees and best practices and
determine limits for the transferability of features between various closely
related and distant learning tasks.