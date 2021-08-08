# Image Crop Analysis

This is an experiment on Twitter's Image Cropping Algorithm, take a look at [the main repository](https://github.com/twitter-research/image-crop-analysis) from Twitter Reserach team. The result was submitted for [Twitter’s first algorithmic bias bounty challenge](https://blog.twitter.com/engineering/en_us/topics/insights/2021/algorithmic-bias-bounty-challenge)


## Title: 
**Gazing at the Mother Tongue: Analyzing Twitter's Image Cropping Algorithm on Bilingual Memes**


### Abstract:
For a moment, I suggest that you stop reading this submission and, instead, take a brief look at your Twitter timeline. How many of the posted tweets are accompanied by text-overlayed images? Memes, GIFs, screenshots, event promotions, and so on? 

By analyzing 200 memes, my submission seeks to understand how Twitter's saliency algorithm crops memes. I conducted experiments to understand whether the algorithm favors Latin scripts over Arabic scripts where both languages are used in a meme and where the algorithm chooses text instead of an object or human body parts. I later describe unintentional harms arising from this biased practice, especially on Twitter users who primarily write in Arabic scripts and/or use Twitter’s Arabic-scripted UI settings. The unintentional harms may include limiting those users' access to information, social and political online participation ("erasure"), and shrinking linguistic diversity online ("under-representation"). I further describe how such biased cropping can be utilized for adversarial use cases where users intentionally decide to hide or highlight parts of a meme (or any images with overlayed-text) and how potential future use cases of the cropping algorithm, in combination with other text extraction and multi-modal algorithms, may hinder Twitter's own efforts in protecting users from harmful content such as hateful speech, xenophobia, and mis/disinformation.        

### Methodology:

#### Experiment 1

For experiment 1, I used [imgflip.com](imgflip.com) to create 3 groups of English-only, Arabic-only, and English+Arabic memes. Each group has 40 memes with unique texts. I first chose 40 English-only memes as "seeds," then translated those texts into Arabic and created 40 identical Arabic memes which maintain the original meme's image ratio, resolution, font size, and color. For the third group I chose to make half Arabic, and half English memes.  

The following images are examples of English-only (marked as *en.jpeg in the data file), Arabic-only (marked as *ar.jpeg in the data file), and English+Arabic (marked as *enar.jpeg in the data file) memes.  


<figure>
<p style="text-align:center;">
<img src="https://github.com/royapakzad/image-crop-analysis/blob/main/data/1en.jpeg" title="English-only Meme" width="300" height="300"/>
<figcaption style="text-align:center;">Fig.1 - English-only Meme.</figcaption>
</p>
</figure>


<figure>
<p style="text-align:center;">
<img src="https://github.com/royapakzad/image-crop-analysis/blob/main/data/1ar.jpeg" title="Arabic-only Meme" width="300" height="300"/>
<figcaption style="text-align:center;">Fig.2 - Arabic-only Meme.</figcaption>
</p>
</figure>

<figure>
<p style="text-align:center;">
<img src="https://github.com/royapakzad/image-crop-analysis/blob/main/data/1bi.jpeg" title="Arabic-only Meme" width="300" height="300"/>
<figcaption style="text-align:center;">Fig.3 - English+ Arabic meme with the same image ratio and characteristics.</figcaption>
</p>
</figure>



After creating 40x3 memes, I used the Twitter saliency algorithm to find the most salient point and cropping preferences for each group of English, Arabic, and English+Arabic memes. The goal of this experiment was 1) to understand whether the text is favored over an object or a body part in a given meme, 2) does this apply to both English and Arabic scripts 3) how this selection preference changes in the case of having both English and Arabic in the same meme with identical characteristics for the English-only and Arabic-only memes such as image size, resolution, color intensity, font color, and font size.

 In the Results section, I describe my findings. You can find the code notebook in the notebook folder under [Meme_Text_Crop.ipynb](https://github.com/royapakzad/image-crop-analysis/blob/main/notebooks/Meme_Text_Crop.ipynb) and full results, including different cropping plots in this [Google folder](https://drive.google.com/drive/folders/1A3-mtde9a6T1mt8Z_x2Hfcf8CbxdlcoQ?usp=sharing). 

#### Results for Experiment 1

The result from experiment 1 shows that in a given meme -- English-only, Arabic-only, and English+Arabic, the text is favored by the algorithm over an object or any human body part in that meme. 39 out of 40 times in English-only memes, the most salient points are on text. In Arabic-only, this is the case only 31 out 40 times. And in English+Arabic memes, and 39 out of 40 times, the most salient points are on a text, with 32 times on English text and 7 times on Arabic text. 

#### Experiment 2
For experiment 2, I combined each pair of Arabic and English images, with English memes on the left and Arabic memes on the right (e.g. 1en.jpeg, 1ar.jpeg resulting in creating 1enar.jpeg image. All 40 combined memes are labeled as *enar.jpeg in the data directory). 

<figure>
<p style="text-align:center;">
<img src="https://github.com/Taraaz-Research/Twitter_Analysis/blob/master/1enar.jpeg" title="Arabic-only Meme" width="600" height="300"/>
<figcaption style="text-align:center;">Fig.4 - Arabic+English joined pair.</figcaption>
</p>
</figure>

 
I then ran the saliency algorithm again and documented the image, whether English or Arabic, that is picked by the algorithm. Because Arabic script is written and read from right-to-left as opposed to left-to-right English text, I also switched pairs’ sides (Arabic meme on the left and English meme on the right, labeled as *aren.jpeg in the data directory) and ran the saliency algorithm again. The goal was to understand whether, independent from the type of script, the saliency algorithm automatically favors the left side of the combined memes because Latin script readers whose gaze scans from the left to the right side of the screen. 

<figure>
<p style="text-align:center;">
<img src="https://github.com/Taraaz-Research/Twitter_Analysis/blob/master/1aren.jpeg" title="Arabic-only Meme" width="600" height="300"/>
<figcaption style="text-align:center;">Fig.5 - Arabic+English combined pair (Ar: Left, En: Right).</figcaption>
</p>
</figure>


#### Results for Experiment 2
For Fig4 type pairs <*enar.jpeg>, the saliency algorithm clearly favors the English region over the Arabic region. 36 out of 40 times (90%) English regions are selected. When English text is on the right side we still see that 37 out of 40 times (92%), the English region is selected. Therefore, independent from text position, English regions are favored. 

The full result from experiment 2 can be found in the <Ar_En_Paired> and <En_Ar_Paired> folders here: https://drive.google.com/drive/folders/1A3-mtde9a6T1mt8Z_x2Hfcf8CbxdlcoQ

### Discussion 
For more than a decade, intergovernmental bodies such as UNESCO have warned governments and Information and Communication Technology companies about the digital linguistic gap. In a book chapter entitled [The Invisibility of Linguistic Diversity Online](https://www.cambridge.org/core/books/bridging-linguistics-and-economics/invisibility-of-linguistic-diversity-online/2231255467B5AF796819EF63098259B8), linguistic anthropologist Ana Deumert raises concerns about the hegemony of the English language online resulting in the rise of digital social and economic inequalities. 

Arabic is the third most written script in the world. More than 660 million people write in variations of Arabic scripts including Arabic, Persian (Farsi/Dari), Uyghur, Kurdish, Punjabi, Sindhi, Balti, Balochi, Pashto, Urdu, Kashmiri, Rohingya, Somali, and Mandinka. Despite this, there are still shortcomings when it comes to content moderation online for communities who choose to use Arabic-script UI settings and communicate in their mother tongue online. Cropping algorithms that favor Latin script over Arabic may result in further under-representation of the above languages -- and hence the associated communities -- online. This may contribute to limiting users’ access to information, hindering those users’ political, economic, and social outreach, especially in cases of using satirical and political memes and/or text-overlayed images to promote events and disseminate vital bi-lingual information in cases of emergencies and crises. In addition, such under-representation and erasure implies that certain identities and languages are valued less than others. Arabic-script users who are predominantly from South and Southwest Asia and North Africa already bear the brunt of social media companies’ over or under-enforcement of automated content moderation practices, especially during those companies’ efforts in countering violent extremism and terrorism (for a reference read the following [article](https://www.al-monitor.com/originals/2021/05/how-egyptians-are-using-ancient-arabic-script-evade-censors) and this Human Rights Impact Assessment [report](https://www.bsr.org/reports/BSR-GIFCT-Report.pdf) about GIFCT). 

After reading the background studies that informed designing Twitter’s Image Cropping Algorithm, there was no surprise that the Arabic script is less favored in cropping algorithms than Latin scripts. According to the Twitter paper (citation 41 and 42), the cropping algorithm is trained on [Salicon Dataset](http://salicon.net/), [MS COCO](https://cocodataset.org/#home), [LabelMe](http://labelme2.csail.mit.edu/Release3.0/#content-inner-3), and [Flickr](https://www.flickr.com/). These are predominantly object-only images with no overlaid texts -- let alone Arabic script text -- which are labeled by Mechanical Turkers. There is a body of survey-based studies ([here](https://dl.acm.org/doi/10.1145/1753846.1753873) and [here](https://www.pewresearch.org/internet/2016/07/11/turkers-in-this-canvassing-young-well-educated-and-frequent-users/)) that show Mechanical Turkers are mainly from the United States or India and use English as their most commonly spoken and written language. Further studies are necessary to understand how people’s reading habits (L to R vs R to L) may trick eye-tracking tools that are commonly used in saliency detection algorithms. 

**Accessibility and UX/UI**
It is also unclear whether Twitter’s cropping algorithm is responsive to the UI’s language setting of Twitter phone and web versions, locations where users tweet from, locations that users publicly declared that they are from, or the language they use in their original tweet. Whether the algorithm should be responsive to language settings or not is out of the scope of this analysis, but it is important for Twitter UX/UI research and localization teams to understand users’ preferences and needs during the development and deployment of such features. 

**Future use cases in multimodal learning** 
Seemingly, at the moment, the Twitter cropping algorithm is solely used for cropping images. However, with a growing interest in using multimodal learning in content moderation practices, there may be a case for the Twitter machine learning research team to consider using a combination of object detection, text extraction, sentiment analysis, and saliency algorithms in their efforts in moderating harmful content. In this scenario, the harms arising from underrepresentation, erasure, and mis-recognition of certain languages will be compounded and may have more drastic impacts. 

I’m closing this section with a quote from [*Memes to Movements*](https://www.penguinrandomhouse.com/books/567159/memes-to-movements-by-an-xiao-mina/): “on the long, winding road from innocuous cat photos, internet memes have become a central practice for political contention and civic engagement.” Considering the importance of memes and the fact that the majority of images attached to Tweets contain text, my analysis concludes that from a technical standpoint, Twitter's cropping algorithm is far from being equipped to understand the context of text-overlayed images, especially in non-English or multi-lingual settings. 

### Limitations
**Small dataset**: In my experiment, I only used 40 commonly-used “seed” memes to create (overall) 200 memes to feed the algorithm and conduct qualitative and quantitative analysis. Although the results have shown a significant difference in favoring English text over Arabic text, further analysis with a larger dataset is recommended. 

**Controlled experiment**: Despite the fact that I tried to keep other factors such as image size, color density, text position, font size, color, opacity, etc. the same, I still think there needs to be a more controlled experiment to come up with definitive answers about biases of the cropping algorithm in multi-lingual text-overlayed images. 

**Cursive writing**:  Both Arabic and English texts are typed in non-cursive fonts. More experiments can show whether cursive or other font forms will change the algorithm preference in cropping the English side or the Arabic side. 

**Qualitative analysis**: Most qualitative analyses on social media experiences need a form of semi-structured or unstructured interviews with actual users. My own experience as a native Farsi writer/reader with intermediate knowledge of Arabic was helpful in understanding common experiences. However, more shared stories are required to understand the types and magnitude of actual harms. 

### Evidence/Reproducibility:
Python Code: https://github.com/royapakzad/image-crop-analysis/blob/main/notebooks/Meme_Text_Crop.ipynb
The full data folder and data_plot folder can be found on Google Drive below. Because of GitHub's 25Mb limit, I only added 7 image files to the data folders on GitHub forked repository. You can download the full data folder and run it locally.
Supporting Material/References:
* data folder (including all English, Arabic, Arabic+English memes in addition to combined paired images): https://drive.google.com/drive/folders/17N70cngHv5_SGe9vBQMso7EaODHBjmsK?usp=sharing
* data_plot folder showing the results: https://drive.google.com/drive/folders/1A3-mtde9a6T1mt8Z_x2Hfcf8CbxdlcoQ?usp=sharing
* Python notebook with a completed run and all outputs: https://drive.google.com/file/d/1w5xxtjiy3nuHr_96h53u0_jA2jpC38Eh/view?usp=sharing.
* pdf version of this Readme submission: https://drive.google.com/file/d/1l-dU0MAd_07eJRHmorcHfqXvnOW4JVoe/view?usp=sharing


### Self-Grading Recommendation: 
Description of Harm
* Harm Base Score:** [20]  [This is unintentional underrepresentation harm affecting Twitter users who communicate mainly in Arabic-script languages. The harm may also include erasure and mis-recognition. ]
* Affected Users: ** [1.2]  [There are no public statistics about the exact number of Twitter users who use Arabic-script languages. However, there are more than 660 million people in the world who write in Arabic scripts. Therefore I chose 1.2 as the multiplying score]
* Likelihood or Exploitability: ** [1.1]  [I gave a score only for the likelihood of the issues described above happening monthly for Twitter users when the actual harm is visible. A brief explanation on exploitability: in intentional exploitation one might use biased cropping to intentionally hide or highlight part of the text or objects in memes and text-overlayed images by adding hateful Arabic text that may go unnoticed, adding/removing/changing watermark position and color intensity, and other forms of artistic modification]
* Justification: ** [1.25]  [In the discussion I cited various literature supporting why paying attention to linguistic diversity is important in the context of cropping memes and other text-overlayed images. ]
* Clarity: ** [1.25]  [I added a limitations section describing the scope of the experiment and what is not included. In other sections, I made concrete recommendations for Twitter’s teams to build on this trial study.]
* Creativity: ** [1]  [I used standard data-based input/output testing that is a common method in auditing algorithms. However, the emphasis on memes, while not entirely novel, does show a forward-thinking approach.]
* Total Score: ** (20)x[1.2+1.2+1.1+1.25+1.25)



