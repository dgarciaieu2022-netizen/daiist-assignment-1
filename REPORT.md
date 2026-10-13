# Assignment 1 Report

- **Name**: Daniel Garcia Laya 
- **Student ID**: 19122
- **Email**: dgarcia.ieu2022@student.ie.edu
- **Group**: BBADBA 5B

## Dataset

I use Inside Airbnb (insideairbnb.com/get-the-data) a project that provides free quarterly scrapes of every Airbnb listing in a city licensed under CC BY 4.0. I picked Valencia. Downloaded the detailed listings file for three consecutive quarters. The March snapshot was scraped on 28 March 2026. Contains 4,930 listings. The June snapshot was scraped on 3 July 2026. Has 7,851 listings. The September snapshot was scraped on 25 September 2026. Includes 7,968 listings. All three datasets have the same 90 columns.

One row represents one listing as it appeared on the day of the scrape. It includes the price, availability, reviews, room type, location, amenities and host details. The files are around 10 MB in total so they fit well in this repo. This means the entire pipeline can be rebuilt from zero if needed.

There is no target variable in the data so I created one: whether a listing is still on the platform in the next snapshot. I chose this dataset because Airbnb is a marketplace, which matches my thesis topic perfectly. Also keeping hosts is a real business challenge, for any platform that relies on supply. The dataset also requires feature engineering because many useful columns come in the form of text, dates or coordinates.

Two data issues I noticed on. First "host_since" is empty in all three snapshots so I use "hosts_time_as_host_months" to measure host experience. Second the March snapshot has 3,000 fewer listings than the July snapshot. The share of listings that leave is similar in both periods, 6.2% in March and 7.3% in July, so I interpret this as growth before summer rather than a scraping issue.. It does mean that the training quarter is smaller, than the test quarter.

## Business / real-life framing

**Scenario.** The host success team at Airbnb in Valencia wants to know, every quarter, which listingsre likely to leave the platform before the next quarter. That way an agent can call those hosts. Help with pricing, availability or reviews. I assume that one call costs twenty euros in staff time and keeps one in three hosts from leaving. When a host leaves Airbnb loses its commission on that listings bookings. I estimate that loss as one quarter of the listings revenue over the year (estimated_revenue_l365d divided by four) multiplied by a fifteen percent commission. Both numbers are assumptions. They make the decision concrete. A call is cheap compared to losing a host and it is not worth the call for a listing that earns almost nothing.

**Target.** Inside Airbnb publishes a scrape of every Valencia listing each quarter. A listing is counted as having left (target equals one) if it appears in one snapshot and is missing from the snapshot and is counted as having stayed (target equals zero) if it appears in both snapshots. I build this target myself so it has a weakness. A host who hides a listing for a while also appears to have left. Between March and July six point two percent of listings left and, between July and September seven point three percent of listings left.

**Split.** I split by time not randomlly.
The model learns from listings as listings were in March and whether listings were gone by July and the model is tested on listings as listings were in July and whether listings were gone by September. That is how the team would really use the model: the model is trained on the past. Applied to a quarter the model has never seen. A random split would mix both quarters so the model would be graded partly on the period the model learned from. Would look better than the model really is.

**Threshold.** Accuracy is the measure because so few listings leave: a model that predicts nobody leaves is already 92.7 % accurate and helps no one. Instead I choose the decision threshold that gives the total cost on a validation set taken from the training period. Total cost is €20 for every host we call. The full lost commission for every host who leaves without a call plus two thirds of the lost commission, for hosts we call but still lose. I look at the test quarter once at the end to report the final result.

## Data preparation & feature engineering

*What you engineered and why, and any data-quality decisions you made along
the way — e.g. "segment X had defective data, so I excluded it and used a
population-average default for scope Y at inference time; the impact of
that choice is Z."*

## Modeling: three implementations, one model

*Which model (linear or logistic regression) and why. A results table
comparing scikit-learn, the manual PyTorch loop, and the standard
torch.nn.Module/torch.optim workflow, on the same test set, against the
naive baseline. Do the three agree? If not, why not?*

## Limitations & next steps

*Real limitations you found, and concretely how you'd address each one with
more time or data — not generic hedging.*

## Generative AI use disclosure

*Per the syllabus AI Policy: what you used and how, or "no AI content used."*