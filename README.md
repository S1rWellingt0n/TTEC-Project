My TTEC project

Test phone number - +1 866-582-2981
Vanity number lookup site to enter phone number without country code. Ex. 9403895512 https://vanitynumbercloudfrontwebsite414.s3.amazonaws.com/index.html

The main challenge was the lambda to generate vanity numbers, trying to get as many possible word matches. First, I tried to do this on my own (Blindly), but unfortunately, I could not do it because of lack of time. After some searching, I found someone who used convert dictionary words into numbers then using a inverse method and matched them. I went with this due to the abiltiy to match each word to a set of numbers. Also allowing the use of l33t and phontic sounding letters if needed. (Also learned abit of the panbdas library for Pyhton.)

https://codereview.stackexchange.com/questions/234213/converting-phone-numbers-to-words-with-python

The speed of the Lambda improved, so that's one less point I would have to be worried about regarding the Lambda taking longer than the timeout allowed on Amazon Connect. I was going to work around this by using a helper lambda to trigger the actual lambda and fetch the data back later.

Not all phone numbers are the same however. I worry some numbers will not bring back 5 matches. I worry without error handling this will break the IVR with certain phone numbers. This is something I would work on more if this were a real project with a bit more time. 

I've sorted by "best" how long the word that was matched. Longer = better, then having the lambda put those words into a DB for later use.

The caller would move along to the next Lambda that would grab the data the first lambda put into the DB. I picked this design so the data could be grabbed by a backend lambda as well for a hosted site. Also, it would allow, if desired, the customer to enter a different number than their own. Incorporating a Lex bot and Lex slot types.

It would grab the DynoDB record and play it back ranked 1 through 3 and give the definition of the matched word. My understanding of a vanity number is a number that has a certain feature which helps in the recollection of the number, and by providing the definition of the word match would help in this regard.

From the get-go, I already knew that contact flows are just big Json files with all the block data stored in them and saw the arns for the lambdas and setup the lambda block arns as parameters to be filled by the cloud formation script itself, nicely getting around that challenge. Then only to add other assets for the Contact Flow and the Lambdas they will need. Being careful to layer the logic to create the lambda first to use the arns in the contact flow later. If I had more time, which would allow for refinement of the deployment scripts and S3 bucket sharing for whoever deploys them before hand. 

Shortcuts taken that I would smooth out would be necking down the APIs to only private use. Or formatting the data so the +1 (Country code) is not taken out when it came to the website or storing data in the DB. Lambdas missing error handling and allowing matching with every single word available that is included in the words file, even non-appropriate words. For high traffic use, I would just provision everything to the expected call volume + 50%. Since most AWS services are on demand, including Lambda, DynoDB, S3, Cloudwatch logs, Connect, just having the provisioned capacity goes along with it. From there, I would then add in Cloudwatch alarms to keep my eyes on everything (I.E the provisioned capacity)(Lambda error rate) and ensure that the API keys are appropriately stored in the AWS secret and that API access is only through an approved IPs. Also adding time to live in the DB and Cloudwatch logs. Removing records after a set number of days.
