# Evaluation

The reason to put **Evaluation** ahead of the model architecture is because, for a generation task, it is imperactive to understand what objective metrics can be used for evaluating the diversity, overal quality, relevance, as well as other perspectives of music generation performance. Moreover, understanding the limitation of these objective metrics and faciliting them with subjective evaluation methods yield a lot, as music generation is ultimatly evaluated by **hearing** instead of numbers.

## Listening Test

The subjective listening test is the most effective method to evaluate the performance of music generation models. Based on the scenarios created for the listening test from the speech generation aspect, MOS (Mean Opinion Score) {cite}`lamere2008social` and MUSHRA (Multiple Stimuli with Hidden Reference and Anchor) {cite}`lamere2008social` become two methods that are frequently applied in the subjective listening test of audio generation.

### MOS Test (Mean Opinion Score)

The purpose of the MOS Test is to verify the overal quality of a **single audio stimulus**, which has been highly applied in the text-to-speech generation task {cite}`lamere2008social` and previously in telecommunications and audio codecs systems. The setup of MOS test is low-cost and simple, where the testers are asked to rate each audio stimulus typically from 1 (bad) to 5 (excellent) via their perception of audio quality, or other specific standards.  

The strengths of the MOS Test is that the whole test is ideal for situations where the overall subjective quality of a single piece of audio needs to be mesured, instead of the comparison among different models or systems. The weaknesses lies in its feedback, which is less sensitive to small quality differences between audio stimulis, and it does not provide insights into the reaons behind the rating. 

### MUSHRA Test (Multiple Stimuli with Hidden Reference and Anchor)

Different from the MOS Test, the MUSHRA Test is regarded as a more advanced method designed for detailed evaluation of multiple audio stimuli and systems. 

The setup of MUSHRA requires the testers to listen to several versions of the same audio signals, including a groundtruth or high-quality version (hidden reference) and a noisy stimulis or low-quality versin (anchor). The testers are asked to rate each stimulus on a continuous scale from 0 (bad) to 100 (excellent) via the standard of the audio quality. The MUSHRA test is frequently designed to evaluate different model abaltions, especially whose difference is subtle. 

The strengths of MUSHURA gurantees a more detailed and sensitive evaluation to quality difference between audio stimuli, then between different models. The inclusive of the reference and the anchor can further help verify if participants can give more accurate responses. Apparently, the weakness of MUSHURA lies in its design expense, which is more complex and time-consuming compared to MOS. Additionally, in a MUSHURA test, participants usually need to evaluate multiple stimuli for each audio sample, which may lead to fatigue over long sessions.