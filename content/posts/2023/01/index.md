---
title: “Look Good to Me — Approve”
subtitle: The Chrome extension that approves Gitlab merge requests with a friendly comment
date: 2023-01-29T09:19:42+01:00
image: 1_JHxNSxdzMwDcabLzhlTyCw.png
draft: false
categories: [Software Development]
tags: [Pull Request, Programming, Code Review, Gitlab, Chrome Extension]
medium: https://medium.com/geekculture/look-good-to-me-approve-4cbed2450a0e
---

{{< figure src="1_JHxNSxdzMwDcabLzhlTyCw.png" caption="https://chrome.google.com/webstore/detail/lgtm-approve/odeollamfjdmamamonbfigajkhakcmag/" >}}



> A chrome extension that automatically adds a comment while approving the request. Doing so cements the collaboration between the reviewer and the merge request’s submitter
> 
> The comment is defaulted to “LGTM” (Look Good to Me) and can be customized.

The extension is available on Chrome Web Store at [https://chrome.google.com/webstore/detail/lgtm-approve/odeollamfjdmamamonbfigajkhakcmag/](https://chrome.google.com/webstore/detail/lgtm-approve/odeollamfjdmamamonbfigajkhakcmag/)

# How it works

The extension works by replacing Gitlab’s “Approve” or “Approve Additionally” button with an upgraded button that adds a comment (e.g. “LGTM”) to the merge request while approving the request.

```javascript
// get the Approve button  
approveBtn = getApproveBtn();  
approveBtnSpan = approveBtn?.querySelector('span');  
approveBtnSpan.textContent = lgtmLabel;  
  
// attach appropriate click's handler  
approveBtn.addEventListener('click', approveBtnClickListener);  
  
// add LGTM comment: the approveBtnClickListener  
const commentTA = document.querySelector('#note-body');  
commentTA.value = text;  
commentTA.dispatchEvent(new Event('change'));  
  
setTimeout(() => {  
  const commentBtn = document.querySelector('div[data-qa-selector=comment_button]>button');  
  commentBtn.click();  
}, 500);

```

As shown in the above excerpt (source: [GitHub](https://github.com/geraldnguyen/lgtm-approve-chrome-ext/blob/main/scripts/content.js#L42)):

*   The extension first gets hold of the Approve (or Approve additionally) button, then update its label.
*   It then sets up and attaches appropriate “click” event handlers to the button.
*   The `approveBtnClickListener` code is shown next. It finds the comment text area, adds the new comment, fires a “change” event, then triggers a “click” event on the Comment button.

There is no need to overwrite the “Approve” action provided by Gitlab UI as the handler responsible for that action is still intact. It will execute as soon as the modified Approve button is clicked.

{{< figure src="https://miro.medium.com/v2/resize:fit:520/1*SDMe512qBknP3m55zwmV0Q.png"  caption="The extension in action (screenshot)" >}}



# Customizing the comment’s text and the button’s label

The extension allows users to customize the comment’s text and button label.

By clicking on the extension icon at the top right corner, a small popup will appear with 2 textboxes for a user to enter new the comment’s text and button’s label

{{< figure src="https://miro.medium.com/v2/resize:fit:291/1*sBlnBf1FBGNRSG3euyzi7A.png"  caption="Customization support (screenshot)" >}}

# “It’s not a bug, it’s a feature”

If a user already approved the pull request, the extension does not keep track of the pull request’s status. If the user later revokes earlier approval, and Gitlab automatically changes the button to “Approve”, it will remain so. Clicking on the button does not automatically add a new comment to the pull request.

{{< figure src="https://miro.medium.com/v2/resize:fit:327/1*xXZVQI3Zm7aZktHiZRhLKw.png" >}}


I believe that scenario like the above deserves a real comment by the user. Scenarios like the above are, by design, not supported.

# Why did _I_ develop this extension?

> _Dedicated to a young colleague who was laid off in Jan 2023. In one of my friendly chats with him, he reminded me that approval alone was not enough._

Together with version control and feature branching, creating pull requests and reviewing code are essential tasks in modern software engineering.

> In my opinion, [code review should be integrated into interviewing process](https://medium.com/50ld/code-review-as-an-interview-tool-f57eb66e5da0).

A good code review should identify shortcomings, ask clarifying questions, or praise for good work delivered. The last action, from my experience, is not done often enough.

To be honest, I am guilty of skipping the praise when reviewing codes. My excuse was that by approving the pull request (called merge request by Gitlab), I already expressed my praise. Looking back now, I could have done better. Some practices even call for a simple “Look good to me” or “LGTM” comment to be added along with each approval.

I experimented with [quick action](https://docs.gitlab.com/ee/user/project/quick_actions.html) but felt that the 3-step combination of typing “/approve”, plus typing “LGTM”, plus clicking on the Comment button is rather long. Ideally, we should be able to do all 3 with one action.

With custom quick action [not yet supported](https://gitlab.com/gitlab-org/gitlab/-/issues/25365) _(I was thinking about a "lgmt" quick action)_, I went ahead to create this extension in the meantime.

# Source Codes

{{< external-link "https://github.com/geraldnguyen/lgtm-approve-chrome-ext?source=post_page-----4cbed2450a0e--------------------------------" >}}



If you like this article, please [follow me](https://geraldnguyen.medium.com/subscribe) for more quality content.
