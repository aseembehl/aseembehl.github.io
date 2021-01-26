---
layout: post
title:  "Automated Fact-Checking"
date:   2021-01-19 20:23:36 +0100
categories: technology
---

Idea arises from the recent work titled Catching Out-of-Context Misinformation with Self-supervised Learning by Aneja et al. 

They propose a simple handcrafted rule to detect images where
two captions align with same object(s) in the image; but semantically convey different meanings, then the image with
its two captions is classified as having conflicting-image captions. 

<img src="/assets/fact_check_teaser.jpg" alt="fact_check_teaser" style="float: left; margin: 0 1.5em 15px 0; min-width: 150px; max-width: 100%" />
Here, both captions align with same object(s) in the image; i.e., Donald Trump and Angela Merkel
on the left and Obama, Dr. Fauci, and Melinda Gates on the right. If the two captions are semantically different from each
other (right), they consider them as out of context; otherwise not (left).




<img src="/assets/fact_check_train.jpg" alt="fact_check_train" style="float: left; margin: 0 1.5em 15px 0; min-width: 150px; max-width: 100%" />
The objective during training is to obtain higher scores for aligned image-text pairs (i.e., if an image appeared with the text irrespective of the context) than misaligned image-text pairs
(i.e., some randomly-chosen text which did not appear with the image). 

To this end, they formulate a scoring function to align objects in the image with the caption. Intuitively, an image-caption pair should have a high
matching score if visual correspondences for the caption are present in the image, and a low score if the caption is unrelated to the image. 

<!-- As they explicitly model the object-caption relationship,
the max operator in Eq. 1 implicitly gives us a strong signal
which object(s) were selected to make that decision, thus
providing spatial knowledge. -->

The resulting Image-Text matching model obtained from
training, as described above, provides an accurate representation of how likely a caption aligns with an image.

<img src="/assets/fact_check_test.jpg" alt="fact_check_test" style="float: left; margin: 0 1.5em 15px 0; min-width: 150px; max-width: 100%" />
At test time they take as input an image and two captions; they then use the trained Image-Text Matching model where
they first pick the top k bounding boxes and check the overlap for each of the k boxes for
caption<sub>1</sub> with each of the k boxes from caption<sub>2</sub>. If IoU for any such pair > threshold t, they infer that image regions overlap.
Similarly, they compute textual overlap S<sub>sim</sub> using pre-trained Sentence Similarity model and if S<sub>sim</sub> < threshold
t, it implies that the two captions are semantically different, thus implying that image is used out of context.











<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
