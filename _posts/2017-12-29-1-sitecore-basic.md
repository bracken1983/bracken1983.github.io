---
layout:     post
title:      SiteCore Component How to
subtitle:   介绍如何做一个sitecore的component
date:       2017-12-29
author:     brackenbo
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Sitecore
    - C#
    - CMS
---

> 描述了如何建立一个sitecore的component

## 1. Create project

* In `CMS\NRMA\Feature` copy a project and rename to yours:

    such as make a copy of holidays and rename the folder to holidayInn

* Rename and modify project file

    * Rename `NRMA.Feature.Holidays.csproj` to `NRMA.Feature.HolidayInn.csproj` in `HolidayInn\code`
    * Change <ProjectGuid>{2CABF582-EA79-4F55-AF38-84335CDE3883}</ProjectGuid> to a new guid in `NRMA.Feature.HolidayInn.csproj`
    * Change all `Holidays` to `HolidayInn`

* Modif `Feature\HolidayInn\code\Properties\AssemblyInfo.cs`
    * Change all `Holidays` to `HolidayInn`
    * Modify guid to previous

* Rename and modify serialization file in `Feature\HolidayInn\code\App_Config\Include\Feature`

    * Rename `Feature.Holidays.Serialization.config` to `Feature.HolidayInn.Serialization.config`
    * Change all `Holidays` to `HolidayInn`
    * After previous action, you should add below folders:

            <include name="Feature.HolidayInn.Templates" database="master" path="/sitecore/templates/Feature/HolidayInn" />
            <include name="Feature.HolidayInn.Renderings" database="master" path="/sitecore/layout/renderings/Feature/HolidayInn" />
            <include name="Feature.HolidayInn.Media" database="master" path="/sitecore/media library/Feature/HolidayInn" />

* Create folder `HolidayInn` and Create html file named `HolidayInnDeals.cshtml` in `Feature\HolidayInn\code\Views`:

    the content of `HolidayInnDeals.cshtml`:

        @model Sitecore.Mvc.Presentation.RenderingModel
        <h1> This is holiday inn</h1>

## 2. In Visual Studio

* Open visual studio and open mynrma project.
* On the right of the window, select the solution explorer
* Then right click the Feature folder -> Add -> New Solution Folder, give the project name `HolidayInn`
* After that right click the HolidayInn folder -> Add -> Existing Project
* Right click the NRMA.Feature.HolidayInn project name -> Build -> Publish..

After that you can see sucessful message in the output console.

    2>Publishing folder Views...
    2>Publishing folder Views/HolidayInn...
    2>Web App was published successfully http://nrma.local/
    2>
    ========== Build: 1 succeeded, 0 failed, 4 up-to-date, 0 skipped ==========
    ========== Publish: 1 succeeded, 0 failed, 0 skipped ==========

## 3. In Sitecore Content Editor



### 3.1 In templates -> feature

> Create a folder named `HolidayInn`

#### 3.1.1 Create new template “_HasHoliday”

* create a section “Holiday Inn Content”

* create follow fields in section “Holiday Inn Content”:

    * Title:  Single-line Text (Title = Holiday Title)
    * Image:  Image (Title = Holiday Image)
    * Description: Single-line Text (Title = Holiday Description)
    * KeyPoints: Rich-Text (Title = Holiday Key Points)
    * Price: Number
    * DealFor: Single-line Text (Title = Deal is for, Help Text = i.e. for a family of 4)
    * OfferImage: Image (Title = Offer Image)
    * OfferTooltipText: Single-line Text (Title = Offer Tooltip Text) – is this required as part of the spec?
    * BookingLink :  General Link (Title = Booking Link)


#### 3.1.2 Create new template “_HasHolidayInnList”

* a. create a section “Holiday List Inn Content ”
* b. create follow fields in section “Holiday List Inn Content”:
    * Title (single-line-text)
    * Holiday List:  Treelist (need to set data source – use the site:source method)

### 3.2 In templates -> project -> NRMA -> Content Types

#### 3.2.1 Create new template “Holiday Inn Deal” inherit from “_HasHolidayInn”

* Create standard Values: OPTIONS -> Standard values

##### 3.2.2 Create new template “Holiday Inn Deal List ” inherit from “_HasHolidayInnList”

* Create standard Values: OPTIONS -> Standard values

### 3.3 In renderings -> feature -> HolidayInn

#### 3.3.1 Create a new rendering “ Holiday Inn Deals”

* Add cshtml file for rendering: Path -> %the path of holiday inn html file%

### 3.4 Assign the this renderings in the allowed components for the placeholder setting

* In `/sitecore/layout/Placeholder Settings/Project/NRMA/Page Types/HolidayInn/1 Column Full` -> Edit -> Layout -> Renderings -> HolidayInn -> Holiday Inn Deals

Because we will used 1 Column Full in the holiday page, so the path like this.


### 3.5 In “/sitecore/templates/Project/NRMA/Folders”

#### 3.5.1. Create a new template called “Holiday Inn Folder”

* Assign an appropriate icon

* Create the standard values

* Assign Insert Options to allow only “/sitecore/templates/Project/NRMA/Content Types/Holiday Inn Deals” and  “/sitecore/templates/Project/NRMA/Folders/Holiday Inn Deals”

### 3.7 Add source for content Types

In `Templates/Project/NRMA/ContentTypes -> Holiday Inn Deal List -> __Standard Values -> Holiday Inn List -> selected -> Holiday Inn Folder

### 3.6 Add Component to layout

We will create holiday inn content in holidays layout page for the holiday layout page has already exist.

#### 3.6.1 In “/sitecore/content/NRMA/Holidays/Local Content” -> Insert from template -> Holiday Inn Folder
#### 3.6.2 In Holiday Inn Folder Insert `Holiday Inn Deal` named "Holiday Inn Deal1" "Holiday Inn Deal2" and "Holiday Inn Deal3"

