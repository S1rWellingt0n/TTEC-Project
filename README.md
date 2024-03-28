My TTEC project

Test phone number - +1 866-582-2981
Vanity number lookup site to enter phone number without country code. Ex. 9403895512 https://vanitynumbercloudfrontwebsite414.s3.amazonaws.com/index.html

The main challenge was the lambda to generate vanity numbers, trying to get as much possible word matches or close words like l33t or sounding phonic. First, I tried to do this on my own, but unfortunately, I could not do it because of lack of time. After some searching, I found someone who tried to convert dictionary words into phone numbers using a inverse method and matched them. 

https://codereview.stackexchange.com/questions/234213/converting-phone-numbers-to-words-with-python

The speed of the Lambda improved, so that's one less point I would have to be worried about regarding the Lambda taking longer than the timeout set on Amazon Connect. I was about to work around this by using a helper lambda to trigger the actual lambda and fetch the data back later.

Not all phone numbers are the same however. I worry some numbers will not bring back 5 matches. I worry without error handling this will break some IVR with certain phone numbers. This is something I would add if this were a real project with a bit more time. I've sorted by "best" how long the word that was matched.

Longer = better, then having the lambda put those words into a DB for later use.

The caller would move along to the next Lambda that would grab the data the first lambda put into the DB.

It would grab the record and play it back 1 through 3 and give the definition of the matched word. It is because a vanity number is a number that has a certain feature that helps in the recollection of the number, and I may add some of the wild words that are matched to the number.

Building the IaaC was fun and a lot easier than I thought. From the get-go, I already knew that contact flows are just big Json files with all the block data stored in them and saw the arns for the lambdas and setup the lambda blocks as parameters to be filled by the cloud formation script itself, nicely getting around that challenge. Then only to add other assets for the Contact Flow and the Lambdas they will need. Being careful to add the logic to create the lambda first to use in the contact flow later down the line. If I had more time, which would allow for refinement of the deployment scripts and S3 bucket sharing for whoever deploys them before hand. Shortcuts taken that I would now smooth out would be necking down the APIs to only private use. Or formatting the data so the +1 (Country code) is not taken out when it came to the website or storing data in the DB. Lambdas missing error handling and allowing matching with every single word available that is included in the words file, even non-appropriate words. For high traffic use, I would just provision everything to the expected call volume + 50%. Since most AWS services are on demand, including Lambda, DynoDB, S3, Cloudwatch logs, Connect, just having the provisioned capacity goes along with it. From there, I would then add in Cloudwatch alarms to keep my eyes on everything (I.E the provisioned capacity)(Lambda error rate) and ensure that the API keys are appropriately stored in the AWS secret and that API access is only through an approved IPs.
