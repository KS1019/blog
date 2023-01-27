---
title: Experience at MIXI as Software Engineer Intern (2022 Summer)
date: 2023-01-27T01:40:53.899Z
slug: internship-at-mixi-in-2022
tags:
  - MIXI
  - swift
  - internship
keywords:
  - internship
  - ios
  - swift
  - MIXI
---

Following last summer, I interned at MIXI Inc. as an Software Engineer in the [minimo](https://minimodel.jp) division of MIXI under the internship program called **Dive into MIXI** for the second time.

<a href="https://apps.apple.com/jp/app/minimo-%E3%83%9F%E3%83%8B%E3%83%A2-24%E6%99%82%E9%96%93%E4%BA%88%E7%B4%84%E5%8F%AF-%E7%BE%8E%E5%AE%B9%E3%82%B5%E3%83%AD%E3%83%B3%E4%BA%88%E7%B4%84%E3%82%A2%E3%83%97%E3%83%AA/id719858778?itscg=30200&amp;itsct=apps_box_appicon" style="width: 170px; height: 170px; border-radius: 22%; overflow: hidden; display: inline-block; vertical-align: middle;"><img src="https://is1-ssl.mzstatic.com/image/thumb/Purple123/v4/28/df/17/28df1703-b6bd-db1c-dd52-e7a747ae4213/AppIcon-1x_U007emarketing-0-7-0-85-220.png/540x540bb.jpg" alt="minimo（ミニモ）24時間予約可！美容サロン予約アプリ" style="width: 170px; height: 170px; border-radius: 22%; overflow: hidden; display: inline-block; vertical-align: middle;"></a>

## tl;dr

You all should [apply](https://mixigroup-recruit.mixi.co.jp/recruitment-category/internship/9575/) too!!!

## About the selection process

I was invited to participate in this year's summer internship by the recruiter who helped me last year, and since I felt that I had a very good experience last year myself, I decided to apply again this year. Similar to last year's process, I had an initial interview with the recruiter and then an interview with an engineer, and was accepted for this year's internship as well.

## Benefits, environment, duration, etc.

I received the same benefits as last year, and the duration of the internship was fully in line with my expectations. I was able to work in the office most of the time this year, so I was able to communicate with more people in the team than last year.

## What I worked on during my internship

### Adding optional images when creating new menus

The first task was to improve the functionality of minimo, which has a step for users to register a menu for beauticians (and other practitioners), but the problem was that there was no item to add an image when registering a new menu. This feature used to be a required field, but was removed in order to lower the hurdle for adding an image to the menu. Before the modification, if you wanted to add an image to the menu, you had to go to "Create New" and then "Edit Created Menu", which was a time-consuming process.

The changes I made in the end were very small, but I learned a lot about how to proceed with the task, as I found differences between the design file and the actual implementation, and had to check the details of the specifications.

### Creating Swift Package Manager automatic update workflow

Since the team had been operating a similar mechanism for CocoaPods and RubyGems, they were motivated to use the same mechanism for the libraries managed by Swift Package Manager. They also wanted to migrate as much of the library management to Swift Package Manager as possible, which they knew would be necessary in the future.
The tricky part was that I had to use Xcode's built-in SPM to make this work, but by making good use of `xcodebuild -resolvePackageDependencies`, I was able to achieve the behavior I had envisioned. Finally, I used fastlane's custom lane and run it on Bitrise.

### Improving SwiftLint rules

Minimo used SwiftLint to maintain code quality. So I added new rules that I think will be useful. The following rules were added.

- [explicit_init](https://realm.github.io/SwiftLint/explicit_init.html)
- [trailing_closure](https://realm.github.io/SwiftLint/trailing_closure.html)
- [redundant_nil_coalescing](https://realm.github.io/SwiftLint/redundant_nil_coalescing.html)
- [cyclomatic_complexity(ignores_case_statements = true)](https://realm.github.io/SwiftLint/cyclomatic_complexity.html)
- [xct_specific_matcher](https://realm.github.io/SwiftLint/xct_specific_matcher.html)

Using SwiftLint's automatic formatting feature, I was able to apply the rules relatively easily. I was also able to make a contribution to SwiftLint through these tasks, which was an exciting experience.

### Building a build time measurement system

I built a system to continuously monitor the build time of iOS applications. Since the increase in build time is one of the factors that detracts from the developer experience, and since I had made some changes myself that could affect build time, I thought it would make sense to have a system that continuously measures build time, so I undertook this task. To briefly explain how it works, Bitrise's workflow execution feature executes a custom lane on the code on GitHub. In that lane, I used a command called hyperfine to measure the time it takes to build the app with xcodebuild. The results are then exported to JSON and stored in an AWS S3 bucket, and the JSON stored in S3 is sent to BigQuery via another service called rundeck. Finally, I used a tool called Looker to visualize the data.

This task was a great learning experience for me because I was able to work with technologies that I had never used before. I was also glad that many employees helped me and I was able to communicate with many different people as I went along.

### Refactoring

- Refactoring where compile time is long
  - Separating into multiple variables
  - Adding type information
- Making magic numbers constant
- Run SwiftLint locally only when .swift files are modified
  - Change to run SwiftLint only when .swift file is changed in Build Phase Script

## Final Impressions

This internship was very interesting because I was able to do a variety of tasks related to improving the development environment and gain new experiences. It was also interesting to look back at the code base after a year and see it from a different perspective. I am also glad that I was able to communicate with more people than when I interned last year.