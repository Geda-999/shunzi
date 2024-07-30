// ==UserScript==
// @name         Magai.co 去模糊、移除元素和启用输入
// @namespace    http://tampermonkey.net/
// @version      1.2
// @description  移除Magai.co上的模糊效果，隐藏特定元素，并启用文本输入
// @author       Mahjongmaster88
// @match        https://app.magai.co/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function removeElements() {
        const selectors = [
            '.column.flex.bubble-r-container.baTwaZp1.Group.bubble-element',
            '.baTydaY.greyout',
            '.baTwaQaG.greyout',
            '.column.flex.bubble-r-container.baTwaQaG.CustomElement.bubble-element > .column.flex.bubble-r-container.baTwcaY.CustomElement.bubble-element',
            '.column.flex.bubble-r-container.baTydaY.CustomElement.bubble-element > .column.flex.bubble-r-container.baTwcaY.CustomElement.bubble-element',
            '.column.flex.bubble-r-container.baTydaY.CustomElement.bubble-element',
            '.column.flex.bubble-r-container.baTwaQaG.CustomElement.bubble-element'
        ];

        selectors.forEach(selector => {
            const elements = document.querySelectorAll(selector);
            elements.forEach(element => {
                element.style.display = 'none';
            });
        });
    }

    // 移除模糊效果相关代码
    const observer = new MutationObserver(mutations => {
        mutations.forEach(mutation => {
            if (mutation.type === 'attributes' && mutation.attributeName === 'style') {
                const element = mutation.target;
                if (element.style.filter.includes('blur')) {
                    console.log('移除模糊效果:', element);
                    element.style.filter = 'none';
                }
            }
        });
    });

    const removeBlurEffect = () => {
        document.querySelectorAll('*').forEach(element => {
            const style = window.getComputedStyle(element);
            if (style.filter.includes('blur')) {
                element.style.filter = 'none';
            }

            observer.observe(element, { attributes: true });
        });

        console.log('MutationObserver已启动。');
    };

    removeBlurEffect();

    const bodyObserver = new MutationObserver(mutations => {
        mutations.forEach(mutation => {
            if (mutation.type === 'childList') {
                mutation.addedNodes.forEach(node => {
                    if (node.nodeType === 1) { // ELEMENT_NODE
                        const style = window.getComputedStyle(node);
                        if (style.filter.includes('blur')) {
                            node.style.filter = 'none';
                        }

                        observer.observe(node, { attributes: true });
                    }
                });
            }
        });

        // 每当添加新元素时执行removeElements函数
        removeElements();

        // 每当添加新元素时执行enableTextInput函数
        enableTextInput();
    });

    bodyObserver.observe(document.body, { childList: true, subtree: true });

    // 启用文本输入功能
    function enableTextInput() {
        const textarea = document.querySelector('textarea#prompt');
        if (textarea && textarea.disabled) {
            textarea.disabled = false;
            console.log('文本输入已启用。');
        }
    }

    // 初始执行
    removeElements();
    enableTextInput();
})();
