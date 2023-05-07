---
marp: true
theme: gaia
_class: lead
paginate: false
markdown.marp.enableHtml: true

backgroundColor: #fff

header: <img src='./assets/rocky_linux_logo.svg'> Learning Ansible with Rocky | Introduction
footer: Rocky Linux Academy - Ansible courses
---
<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
blockquote {
  background: #ffedcc;
  border-left: 10px solid #d1bf9d;
  margin: 1.5em 10px;
  padding: 0.5em 10px;
}
blockquote:before{
  content: unset;
}
blockquote:after{
  content: unset;
}
header {
    display: grid;
    grid-template-columns: 1fr max-content;
    background-color: #10b981;
    align-content: right;
    color: white;
    font-size: 1em;
    padding: 20px;
}
footer {
    display: grid;
    grid-template-columns: 1fr max-content;
    background-color: #10b981;
    align-content: right;
    color: white;
}

div.twocols {
  margin-top: 35px;
  column-count: 2;
}
div.twocols p:first-child,
div.twocols h1:first-child,
div.twocols h2:first-child,
div.twocols ul:first-child,
div.twocols ul li:first-child,
div.twocols ul li p:first-child {
  margin-top: 0 !important;
}
div.twocols p.break {
  break-before: column;
  margin-top: 0;
}

</style>

![right:20% w:100](./assets/rocky_linux_logo.svg)

Ansible is a simple, yet powerful, automation engine for Linux.

<div class="twocols">

This tutorial will guide you through the concepts of using Ansible to automate your IT tasks in a way that is (hopefully) fun and informative. 

<p class="break"></p>

Using the exercises throughout these chapters, will help you gain a comfort level with Ansible in real-world applications.

</div>

---

# Table of contents

* [Introduction](./ansible/Learning_Ansible_with_Rocky-0-Introduction.html)
* [Ansible Basics](./ansible/Learning_Ansible_with_Rocky-1-Ansible_Basics.html)
