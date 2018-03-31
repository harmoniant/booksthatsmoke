---
layout: page
title: Data, Methods, and Results
permalink: /datamethodresults/
---

To investigate representations of smoking in fiction, I worked with a subset of the University of Chicago Novel Corpus that contained 240 books published between 1920 and 1990. The books were evenly divided across the decades (a setup which, admittedly overrepresents, the single year from the 90s that is included). All of the books are in English, and most are by American or British.

To begin, I needed to gather samples of passages that contained and did not contain representations of smoking. I assembled the smoking passages manually by randomly selecting a text file and ctrl-f searching for smoking indicator words such as ‘smoking,’ ‘cigarette’, ‘puff’, ‘pipe’, ‘ashtray’, and so on. When one of these terms was found, I read the surrounding text to verify that the word in question was in fact indicative of smoking behavior (as opposed to say water running through a  pipe , or  puffing  out one’s chest). If I verified that the passage depicted smoking, I copied the selection into a spreadsheet, along with the ID number for the text so that I could later match it with the appropriate metadata. I then repeated this process until I had gathered 50 passages.

Collecting the non-smoking text was a bit more complicated. Do do this, I wrote a Python script that gathered the passages in a multi-step process. First, the script would randomly select a text file from the directory that contained the 240 member subset of the Chicago Novels Corpus. Next, it selected a particular word by generating a random number between one and the word count of the text file minus 300. It then collected this word and the next 300 hundred words as a candidate passage.2 The script then checked to see if the passage contained any smoking indicator words. If it did not, the script would be added to the collection of non-smoking snippets. If a smoking indicator word was present, the passage was rejected. This process was repeated until 70 passages were collected.

After gathering these passages, I performed an initial analysis by training a Naive Bayes classifier to distinguish smoking passages from non-smoking passages. While the results varied across different runs, generally speaking the classifier did not do a terribly impressive job at classifying texts correctly. A recent run yielded a 75% level of accuracy, which was on the high side of things. I also ran the model through a five-fold cross-validation, which resulted in an average accuracy of 69.78%. Again, not terribly impressive.

After experimenting with the classifier, I used loggodds and signed_dunnings functions to get a list of words that are over and underrepresented in the two groups (smoking vs. non-smoking),relative to one another. This generated some interesting results. The top value words are, unsurprisingly, words that we would associated with smoking (Note that “ekler” is the name of a character is a novel whose name is mentioned several times within a relatively short passage). The fact that these words are what we would expect suggests that, at least to some extent, the functions are doing what they are supposed to do.

More interesting are the bottom values, which include four words that we might associate with femininity (‘she’, ‘her’, ‘mrs’, and ‘girl), and arguably a fifth (‘beautiful’). These values seem to line up the stereotype of smoking’s being a masculine behavior (see the Marlboro Man) or “unladylike.” They thereby suggest a path for further inquiry, namely, into the relationships between representations of smoking and gender in novels.

As the classifier had performed rather poorly in distinguishing the smoking and non-smoking text-snippets, I decided to try repeating the above procedures with one change. Instead of using the list of smoking indicator words mentioned above, I used only the words ‘cigarette’ and ‘cigarettes’ to divide smoking and non-smoking passages. For this round, rather than compiling the smoking snippets manually, I edited the script I had used for the non-smoking passages earlier to collect passages that contained either of these two terms. Generally speaking, the hope was that by focusing on cigarette smoking, rather than smoking in general, the classifier would have an easier time.

Unfortunately, this hope was not born out as the classifier performed at similar levels. A recent run yielded a 66.47% average accuracy after performing a five-fold cross-validation. Moreover, the top and bottom valued words also seem to be less meaningful.

After this initial poking around with the texts, I decided to see if I could discern anything interesting by looking at changes over time. While I nothing terribly interesting emerged from a comparison of top and bottom word values across decades (see Jupyter Notebook), I did find an interesting downward trend in the frequency of cigarette references over time.

To carry out this investigation, I created a pandas dataframe that included the full text of all 240 novels and merged this with another dataframe that contained the corresponding metadata. I then wrote a script to go through the text of each novel and tally up the number of times that the term ‘cigarette’ or ‘cigarettes’ was mentioned, and add this to a new column in the dataframe. This allowed me to plot changes in how frequently these terms were used over the years. Here we see some interesting patterns. Looking at both the mean and median counts of ‘cigarette’ and ‘cigarettes’, we see a clear decrease in the usage of the terms over the time period covered.

![mean cigarette](https://github.com/harmoniant/booksthatsmoke/blob/gh-pages/mean_cig_count.png?raw=true "Mean Cigarette Count")
Mean Cigarette Mentions

![median cigarette](https://github.com/harmoniant/booksthatsmoke/blob/gh-pages/med_cig_count.png?raw=true "Median Cigarette Count")
Median Cigarette Mentions
