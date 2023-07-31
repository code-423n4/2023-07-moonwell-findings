# Analysis Report by SolSaver

## Codebase Quality Summary
I would grade the codebase quality as high. Most of the methods were taken from Compound protocol, so it was inherent that the best practices in Compound was followed here as well.
The codebase was commented thoroughly, and was relatively easier to understand than the other audits. However, some of the methods were pretty long, and the contracts were just too long to understand everything going on in there. For the new code that has been implemented, there is an opportunity to reduce the contract and method sizes and break it down into more digestible sizes.

## Auditing Approach
I started by reading the documentations in the repository as pointed out on the audit page. Then I skipped through the videos. After that I started to go line by line and started to leave notes for myself to come back. After going through the repository once, I started to go back to my notes, and started to spend more time in those areas that were flagged. In the first pass I eliminated false suspicious. Whatever remained after that, I stared to tackle the easier ones, and then went on to the hard ones. Created POCs and used unit tests to verify the vulnerabilities as needed.

## Architecture Feedback
As this was based on Compound protocol, the overall architecture was pretty standard and sort of known. The new features that were added like the caps and improvement of user features were nice improvements. A lot of the new code architecture was influenced by the Compound protocol architecture, and it got hard to say what was new vs what was coming from Compound (Its a good thing).ing.

## Centralization Risk
All the governance controls seems like a centralization risks throughout the code, as has been pointed out here: https://gist.github.com/liveactionllama/0a27b77de2aa56abd2e215c82a39f86d#m04-the-owner-is-a-single-point-of-failure-and-a-centralization-risk

## Risks
The send rewards functionality seems concerning. No rewards are sent if there isn't enough balance of the emission token, and is just rather kept accrued, which seems like a liquidity risk to me.

## Time spent:
20 hours

### Time spent:
20 hours