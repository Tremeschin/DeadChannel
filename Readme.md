# YouTube Dead Channel Remover

## Description

This **Tampermonkey** script removes _"invalid"_ YouTube channels from the YouTube **Brand Accounts** dashboard, where the one is a **manager of a removed channel**

- This only seem to happen on the **Desktop** version of `https://www.youtube.com`

**Google**, please fix:

> When a user accepts being a channel manager, there's no direct way to opt-out without accessing the managed account. If the channel is removed in the future, the manager can be left in an invalid state, unable to remove the channel from their YouTube Studio dashboard. This script visually removes such edge cases.

## Installation

### Prerequisites

- Install a user script manager extension:
  - [Tampermonkey for Chrome](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
  - [Tampermonkey for Firefox](https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/)

### Installation Steps

1. Install the user script manager extension for your browser if you haven't already
2. Click on the extension icon in your browser toolbar and select "Create a new script"
3. Replace any existing code with the script provided below:

   ```javascript
    // ==UserScript==
    // @name         YouTube Dead Channel Remover
    // @namespace    http://tampermonkey.net/
    // @version      2024-06-27
    // @description  Visually removes managed brand accounts that have been removed
    // @author       Tremeschin
    // @match        *://www.youtube.com/*
    // @icon         https://www.google.com/s2/favicons?sz=64&domain=youtube.com
    // @grant        none
    // ==/UserScript==

    (function() {
        'use strict';

        // Remove non-hidden "#studio-redirect" accounts
        function removeDeadChannels() {
            document.querySelectorAll('ytd-account-item-renderer').forEach(item => {
                if (!item.querySelector('#studio-redirect').hasAttribute('hidden')) {
                    item.remove();
                }
            });
        }

        // Dynamically call the main method as the DOM updates
        const observer = new MutationObserver(removeDeadChannels);
        observer.observe(document.body, {childList: true, subtree: true});
    })();

4. Save the script and reload any YouTube pages
5. Send feedbacks to Google for a proper fix

## Disclaimers
- I'm not a web developer, feel free to improve the current code
- It might break at any moment with changes or new features of YouTube
