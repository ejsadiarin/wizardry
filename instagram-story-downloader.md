---
date: 2024-12-26T21:45
tags: 
- How-To
---
<!-- 2024-12-26-2145 (December 26, 2024 09:45:49 PM) -->

# Instagram Story Downloader in 5 Steps

The below procedure works for me:

1. Right click on the page, go to Inspect. Navigate to the Network tab and start playing the story, or restart if it was already playing.

2. Sort the elements in descending order of size. The top most files you see are the parts of the original `MP4` being downloaded by Instagram.

3. Copy the link address for any of these files and it should look something like this :

`https://scontent.cdninstagram.com/v/t72.122/xxxxxxx.mp4?_nc_cat=109&ccb=1-7&_nc_sid=xxxx&efg=xxxxxxx&_nc_ohc=xxxx&_nc_ht=scontent.cdninstagram.comxxxxxxx&bytestart=772395&byteend=2377111`

4. Remove the part that starts with `&bytestart` until the end and your final link should look like the below:

`https://scontent.cdninstagram.com/v/t72.122/xxxxxxx.mp4?_nc_cat=109&ccb=1-7&_nc_sid=xxxx&efg=xxxxxxx&_nc_ohc=xxxx&_nc_ht=scontent.cdninstagram.comxxxxxxx`

5. Now, copy this new link and paste in a new tab of the same browser and you should be able to download the video file.

ref: [https://www.reddit.com/r/DataHoarder/comments/z3tp9g/comment/kcyh6i4/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button](https://www.reddit.com/r/DataHoarder/comments/z3tp9g/comment/kcyh6i4/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
