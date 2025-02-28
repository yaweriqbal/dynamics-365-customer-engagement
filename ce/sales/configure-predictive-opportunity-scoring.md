---
title: "Configure predictive opportunity scoring"
description: "Configure predictive opportunity scoring to help sellers prioritize opportunities based on scores and achieve higher opportunity qualification rates."
ms.date: 01/10/2022
ms.custom: 
ms.topic: article
ms.assetid: a1d02708-0e40-4967-ae1a-40e9c67186c8
author: udaykirang
ms.author: udag
manager: shujoshi
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
caps.latest.revision: 1
topic-status: Drafting
---
# Configure predictive opportunity scoring 

Configure predictive opportunity scoring to help sellers prioritize opportunities based on scores and achieve higher opportunity qualification rates.

## License and role requirements

| &nbsp; | &nbsp; |
|-----------------------|---------|
| **License** | Dynamics 365 Sales Premium or Dynamics 365 Sales Enterprise<br>More information: [Dynamics 365 Sales pricing](https://dynamics.microsoft.com/sales/pricing/) |
| **Security Role** | System Administrator <br>  See [Predefined security roles for Sales](security-roles-for-sales.md)|
|||

## What is predictive opportunity scoring

Predictive opportunity scoring uses a predictive machine learning model to calculate a score for all open opportunities. The score helps salespeople prioritize opportunities, achieve higher opportunity qualification rates, and reduce the time that it takes to qualify an opportunity.   

>[!NOTE]
>Your historical data will be deleted after 30 days from the date of your subscription expiration. 

Using this score, you can:

- Identify quality opportunities and convert them into won deals.
- Spend time on opportunities that have low scores and convert them into possible deals.

For example, say you have two opportunities&mdash;Opportunity A and Opportunity B&mdash;in your pipeline. The opportunity scoring model applies a score of 75 for Opportunity A and 55 for Opportunity B. By looking at the score, you can predict that Opportunity A has more chances of being converted into a won deal, and you can engage it. Also, you can further analyze why the score of Opportunity B is low by looking at the top reasons that are influencing the score and deciding how to improve this score.

The following image is an example of an opportunity scoring widget.

> [!div class="mx-imgBorder"]
> ![Predictive opportunity score widget](media/predictive-opportunity-scoring-widget.png "Predictive opportunity score widget")

>[!IMPORTANT]
>- If you're using predictive opportunity scoring that pertains to a version prior to 2020 release wave 2 for Dynamics 365, delete the model. Otherwise, the previous version of the model will be applied on all opportunities in your organization, and the newly generated models won't have any effect on the opportunities. More information:[Delete a model](#delete-a-model)
>- From 2020 release wave 2 for Dynamics 365, the application writes the opportunity scoring related data to **msdyn_predictivescore** entity and has stopped writing to the opportunity entity. This entity is common for both lead and opportunity scoring. More information: [Entity reference](entity-reference.md).

You can add custom fields to generate an accurate model for predictive opportunity scoring. The custom fields can be specific to your organization so that you can decide the impact of the outcome.

## Prerequisites

Verify that you meet the following requirement before adding predictive opportunity scoring models for your organization:

- A minimum of 40 won and 40 lost opportunities within the past 18 months.

    >[!NOTE]
    >These numbers represent the minimum requirement. The more opportunities you can include to train the model, the better the prediction results will be.

- Verify that advanced Sales Insights features are enabled. More information: [Install and configure premium Sales Insights features](intro-admin-guide-sales-insights.md#install-and-configure-premium-sales-insights-features). 

## Understand the configuration page

Before we configure the predictive opportunity scoring, let's understand the configuration summary page. When a model is generated and published, the configuration summary page is displayed as shown in the following image.

> [!div class="mx-imgBorder"]
> ![Opportunity scoring configuration page](media/si-admin-predictive-opportunity-scoring-configuration-page.png "Opportunity scoring configuration page")

The configuration page is organized into the following sections:

- [Select a model](#select-model)
- [Actions you can perform on the model](#actions-you-can-perform-on-the-model)
- [Version details](#version-details)
- [Opportunity score grading](#opportunity-score-grading)
- [MultiModel](#multimodel)

<a name="select-model"></a>

### Select model

In the upper-left corner of the page, you can use the **Select model** drop-down list to choose the model you want to view, edit, or delete. The list consists of both published and unpublished models.

> [!div class="mx-imgBorder"]
> ![Select model drop-down](media/si-admin-predictive-opportunity-scoring-select-model-drop-down.png "Select model drop-down")

### Actions you can perform on the model

In the upper-right corner of the page, you can choose from actions that you can perform on the model.

> [!div class="mx-imgBorder"]
> ![Actions for opportunity scoring](media/si-admin-predictive-lead-scoring-buttons.png "Actions for opportunity scoring")

- **Publish**: When you publish a model to your organization, users in your organization can see the My Open Opportunities Scored system view and the opportunity score widget on opportunity forms. After you publish, this button appears dimmed and will be available only after you retrain or edit the model.
- **Edit model**: You can update or add fields that affect the prediction accuracy score. This is useful when you want to modify fields to consider or include a unique business process. More information: [Edit and retrain a model](#edit-and-retrain-a-model)
- **Revert version**: You can return the model to its previous version when the retrained model isn't satisfactory or doesn't meet an acceptable level of your organization's requirements. This action is only available when you've retrained the model but haven't published it yet.
- **Delete model**: You can delete models that aren't required in your organization. This option is displayed for published models. More information: [Delete a model](#delete-a-model)

### Version details

The parameters displayed in this section show information about the status and performance of the model.

> [!div class="mx-imgBorder"]
> ![Version details for the opportunity scoring model](media/si-admin-predictive-opportunity-scoring-version-details.png "Version details for the opportunity scoring model")

| Parameter | Description |
|-----------|-------------|
| Version trained on | Displays a date that lets you know when the model was last trained. |
| Status | Displays whether the model is active or inactive. |
| Attributes used | Displays the number of attributes used from the available list to train the model. If you're not satisfied with the outcome of the trained model, you can select **Retrain with recommended fields** to retrain the model with the standard (out-of-the-box) attributes. If the parameter displays **Edited** next to the number of attributes used, this specifies that the attributes used are custom-selected. |
| Model performance | Displays information about the model's accuracy and projected performance based on the data available and selected to train the model.<br>**Note**: The range of the accuracy score is defined based on the area under the curve (AUC) classification measurements.<br>- **Ready to publish** specifies that the model accuracy is above the range, and you can expect that the model will perform well.<br>- **OK to publish** specifies that the model accuracy is within range, and you can expect that the model might perform reasonably well.<br>- **Not ready to publish** specifies that the model accuracy is below the range, and you can expect that the model will perform poorly. |
| Business process flow | Displays the business process flow that's applied on the opportunities that are scored by this model. |
| Filter column and filter values | When multiple models are used, this selection defines which column and which values within that column correspond to the opportunities that this specific model should score. |
| State option set | Displays the option set that's used for won and lost opportunities in this model. |
| Retrain automatically | Allows you to set the model to be retrained automatically. More information: [Automatic retraining](#automatic-retraining) |
| Most influential fields | Displays the top five attributes that most affect the outcome of the prediction accuracy score. |

### Opportunity score grading

When a model is published, the opportunities that are in your organization's pipeline are graded according to the range defined in this section. Each opportunity in the pipeline is graded A, B, C, or D, according to the opportunity score. Opportunities in the top score range are graded A while opportunities within the lowest score range are graded D.

> [!div class="mx-imgBorder"]
> ![Grading of opportunity scores](media/si-admin-predictive-opportunity-scoring-grading.png "Grading of opportunity scores")

You can configure the range for the grading according to your organizational requirements. When you change the opportunity score range for a grade, the maximum range value for the adjacent grade changes automatically in accordance with the change in the minimum value. For example, when you change the minimum range value score for **Grade A** to 51, the maximum opportunity score range for **Grade B** changes to 50.

### MultiModel

In the lower-left corner of the page, you can use **Add model** to generate a new model to represent a line of business that might use different opportunities than your first model. The **Add model** command will be disabled as soon as you reach the maximum limit of 10 models (both published and unpublished). More information: [Add a model](#add-a-model)

> [!div class="mx-imgBorder"]
> ![Add a model option](media/si-admin-predictive-lead-scoring-add-model.png "Add a model option")

## First-run setup experience

When the predictive opportunity scoring configuration section is opened for the first time in your organization and no models have been trained on the installation of Sales Insights, you must add the model. 

However, if your organization has enough opportunities that match the application default configurations, a model is generated by default and a pop-up window is displayed with the prediction accuracy score and top five fields that are influencing the score. Based on your organization's requirements, you can publish the model, or you can edit and then publish the model.

If you're using your custom attributes for opportunity generation, you can generate the model by configuring the parameters with your custom attributes.

1. Before you configure the model, review the [prerequisites](#prerequisites). 

    > [!NOTE]
    > You can also enable predictive opportunity scoring through quick setup (with the Dynamics 365 Sales Enterprise license). In this case, you'll get 1500 scored records per month. To enable predictive opportunity scoring through quick setup, you must go to the **Get started with digital sales page** under **App Settings**. More information: [Lead and opportunity scoring](digital-selling.md#lead-and-opportunity-scoring)

2. Go to **Change area** in the lower-left corner of the page, and select **Sales Insights settings**.

    > [!div class="mx-imgBorder"]
    > ![Select Sales Insights settings option](media/si-admin-change-area-sales-insights-settings.png "Select Sales Insights settings option")

3. On the site map under **Predictive models**, select **Opportunity scoring**.

    The **Predictive opportunity scoring** configuration page is displayed.

    > [!div class="mx-imgBorder"]
    > ![Predictive opportunity scoring add model page](media/si-admin-predictive-opportunity-scoring-add-model-page.png "Predictive opportunity scoring add model page")

4. In the **New model name** box, enter a name that contains alphanumeric characters. Underscores are allowed, but not spaces or other special characters.

    By default, the name is selected as **OpportunityScoring_**<***YYYYMMDD***><***Time***> (for example, **OpportunityScoring_202009181410**). The date and time are based on Coordinated Universal Time (UTC).

5. In the **Business process flow** list, select a business process flow that's relevant for the opportunities for which you're generating the model. The values displayed in the list are the business process flows that are defined for opportunities in your organization.

    A business process flow defines a set of steps that users can perform to achieve an outcome. An organization can have multiple business processes flows to represent the work of different security roles or lines of business.

    You must enable custom business process flow entities for analytics and to be able to select them. More information: [Define entities for analytics](#define-entities-for-analytics)

6. In the **State option set** list, select the option set in which the status of the opportunities is defined, and then select the corresponding won and lost values in the **Won value** and **Lost value** lists, respectively.

    Opportunities are marked as:
    - **Won** when they've met certain criteria that denotes that they are ready to purchase a product or service from a business.
    - **Lost** when the criteria aren't met. 

    The out-of-the-box **Status** state option set contains the won and lost values as **Won** and **Lost**, respectively. You can also select your custom option set that's relevant to your business.

7. Select **Filter column** and **Filter values** to define the opportunities for which the model must score.  

    With multiple models, each model can be directed to score a specific set of opportunities based on the line of business they belong to, or based on other criteria. The filter column is the column that holds the value that distinguishes which opportunities the model should score. These selections determine which column and which values within that column correspond to the opportunities that this model will score.  

8. Choose time period from the **Train with opportunities from the past** list. The default duration is 2 years.   

   For example, if you want to consider the closed opportunities from past 6 months to train the model, choose 6 months from the list. The model considers the closed opportunities from the past 6 months and trains the model to score the open opportunities from past 2 years.  

   If you don't have enough closed opportunities in the chosen time period, the **Get started** option will be disabled. Choose another time period that has enough closed opportunities to train the model.  

9. Select **Get started**. 

    The application starts generating a model, and a notification is displayed. The application uses the standard attributes to generate the model.

    > [!div class="mx-imgBorder"]
    > ![Model training notification](media/si-admin-predictive-lead-scoring-model-training-notification.png "Model training notification")

    >[!NOTE]
    >If there aren't enough opportunities to generate the model, an error message is displayed. Review and edit the configurations, and try generating the model again.

10. After the model is generated, the opportunity scoring configuration page is displayed with the version summary, including model performance, the top fields that are influencing the outcome, and the option to choose to automatically retrain the model. 

11. Select **Publish**, if the accuracy of the score is at an acceptable level in accordance with the standards of your organization.

    The model is applied to the selected set of opportunities in your organization. Users can see the opportunity scoring in their views under the **Opportunity score** column and a widget in the opportunity form. More information: [Convert leads into opportunities](../sales/work-predictive-opportunity-scoring.md)

    >[!NOTE]
    >If the accuracy of the score isn't acceptable, select **View details**. You can review the details of the model and edit the fields to improve the score's accuracy. More information: [Edit and retrain a model](#edit-and-retrain-a-model)

## Add a model

In organizations that have different lines of business, you might need different models to score the corresponding opportunities. To accomplish this, you can add and publish multiple models that are specific to each line of business in your organization. To ensure that these models are accurate for your organization, you can choose custom attributes (fields) to be used to generate the opportunity score for a model.

1. Go to the predictive opportunity scoring configuration summary page.

2. In the lower-left corner of the page, select **Add model**.

    > [!div class="mx-imgBorder"]
    > ![Select add model](media/si-admin-predictive-opportunity-scoring-model-select-add-model.png "Select add model")

    >[!NOTE]
    >If you already have 10 models (both published and unpublished), the **Add model** option is disabled. Delete the models that are no longer required in your organization.  More information: [Delete a model](#delete-a-model)

    > [!div class="mx-imgBorder"]
    > ![Add model page for predictive opportunity scoring](media/si-admin-predictive-opportunity-scoring-model-add-model-page.png "Add model page for predictive opportunity scoring")

3. Perform steps 4 through 9 in [First-run setup experience](#first-run-setup-experience), earlier in this topic, to add the model. 

4. After the model is generated, a confirmation message appears with a summary of model performance, the top fields that are influencing the outcome, and the option to choose to automatically retrain the model. 

    > [!div class="mx-imgBorder"]
    > ![Model training confirmation notification](media/si-admin-predictive-opportunity-scoring-model-confirmation-notification.png "Model training confirmation notification")

5. Select **Publish**, if the accuracy of the score is at an acceptable level in accordance with the standards of your organization.

    The model is applied to the selected set of opportunities in your organization. Users can see the opportunity scoring in their views under the **Opportunity score** column and a widget in the opportunity form. More information: [Prioritize opportunities through scores](../sales/work-predictive-opportunity-scoring.md)

    >[!NOTE]
    >If the accuracy of the score isn't acceptable, select **View details**. You can review the details of the model and edit the fields to improve the score's accuracy. More information: [Edit and retrain a model](#edit-and-retrain-a-model)

## Edit and retrain a model

It's time to retrain a model when its prediction accuracy score doesn't meet your organization's standards, or the model simply gets old. Retraining generally increases the prediction accuracy score. The application uses the latest data (opportunities) from your organization to train the model so that it can provide more accurate scores for your users.

>[!NOTE]
>For better prediction accuracy scoring, retrain a model after the data in your organization is refreshed.

You can retrain the model [automatically](#automatic-retraining) or [manually](#manually-retraining). Both methods are described in the following sections.

### Automatic retraining

Automatic retraining allows the application to retrain a model automatically once every 15 days. This can allow the model to learn from historical data and improve its prediction accuracy over time. Depending on the model's accuracy, the application makes an informed decision whether to publish or ignore the retrained model.

To retrain a model automatically, go to the predictive opportunity scoring configuration page and select **Retrain automatically**. By default, this option is enabled when a model is published. Here are the scenarios where the application automatically publishes the model:

- When the accuracy of the retrained model is equal to or greater than 95 percent of the accuracy of the active model.
- When the current model is more than three months old. 

    >[!NOTE]
    >A retrained model might not be published if the accuracy of the model isn't maintained at the application's standard. If this occurs, the existing user-published model will be retained.

### Manually retraining

1. Go to the predictive opportunity scoring configuration page, and select **Edit model**.

2. On the **Edit fields** page, select attributes from opportunity entity, and its related entity (account) including custom attributes to train the model.

    > [!div class="mx-imgBorder"]
    > ![Edit model page](media/si-admin-predictive-opportunity-scoring-edit-model-page.png "Edit model page")   

    >[!NOTE]
    >The scoring model don't support the following types of attributes:
    >- Attributes on custom entities
    >- Date and time related attributes
    >- System generated attributes (such as, opportunityscore, opportunitygrade, version number, entity image, exchange rate, and predictive score ID)


3. Select **Retrain model**.

    The model starts to be generated with the selected custom attributes, and a notification is displayed on the screen.

4. On the configuration summary page, review the model performance and other parameters. If the retrained model satisfies your organizational requirements, publish the model.

    >[!NOTE]
    >If the parameters of the retrained model aren't satisfactory, edit the attributes and retrain the model. 

## View attribute insights

On the **Attribute Insights** pane, you can view detailed information about an attribute, such as its qualification rate and the most important reasons&mdash;both positive and negative&mdash;for that rate. This information provides insights on the performance of each attribute that influences the prediction score. Based on these insights, you can analyze and understand:

- Why certain attributes carry more prediction influence than others.
- How the attribute values compare to the attribute global qualification rate.
- How the model harnesses your data to drive predictive scores.

Additionally, you can connect the attribute value's relative impact on the score to the data input behaviors of your sellers and how that might affect the accuracy of the predictive score.

The insights displayed on the **Attribute Insights** pane are based on your organization's opportunity data and how it correlates to qualified outcomes. For example, when an opportunity has an attribute value that correlates with a qualification rate above the attribute's global qualification rate, the predictive score of that opportunity increases. When the qualification rate for an opportunity is below that of the attribute's average, the predictive score decreases.

The following image shows an example of an **Attribute Insights** pane for the **Annual Revenue (Base)** attribute.

> [!div class="mx-imgBorder"]
> ![Attribute Insights pane](media/si-admin-predictive-opportunity-scoring-attribute-insights-pane.png "Attribute Insights pane")    

Typically, the **Attribute Insights** pane is divided into the following sections:

- A summary of the status of the prediction influence, how many times the attribute is populated in open and closed opportunities, and the reason the attribute isn't automatically selected to create the model.

- A graph that illustrates how each value of the attribute contributes to the qualification rate. In this example, you can see that the opportunity score value **High** perform better than the average, and **Blank** perform below the average. The average is represented by a blue line and calculated based on the following formula:

  `Global qualification rate` = {`Total number of opportunities qualified in your organization`/(`Total number of qualified + disqualified opportunities through this attribute`)} &times; 100

  Hover over each bar to view the summary of the value, such as the qualification rate and the number of open and closed opportunities. The qualification rate for a value of the attribute is calculated based on the following formula:
 
  `Qualification rate for a value of the attribute` = (`Total number of opportunities qualified with the given value in the attribute`/`Total number of closed opportunities with that value in the attribute`) &times; 100
 
  For example, if opportunities with high budget have a 42 percent qualification rate, the formula is:

  (`Total number of opportunities with high budget that are qualified)/( Total number of opportunities with high budget that are closed`) &times; 100 = 42  

  >[!NOTE]
   >These calculations are based on the sample data at the time the model is trained, and might not represent the current snapshot of data. The past two years of data is considered and if the model has a filter, the calculations are done after the data is filtered.  

- A **Details** section that provides reasons for why the values are trending as they are in the graph at that point in time. If there isn't enough data for attributes from related entities, the application won't display the insights.

- The **About** tab provides more information about the attribute insights.

>[!NOTE]
>The insights for the attributes are updated when the model is retrained, either manually or automatically. For models that were created before March 2021, data for attribute insights won't be available. We suggest that you retrain&mdash;or enable the option to automatically retrain&mdash;these models to view the attribute insights.

**To view the Attribute Insights pane**

1. Go to the predictive opportunity scoring configuration page, and select **Edit model**.

2. On the **Edit fields** page, select the attribute for which you want to view insights, either from **Primary entity** or **Related entities**. The **Attribute Insights** pane is displayed on the right side of the page.

## Delete a model

You can delete a model when it's no longer required in your organization. You can have only 10 models&mdash;both published and unpublished&mdash;simultaneously.

1. Go to the predictive opportunity scoring configuration page.

2. Select a model from the **Select model** list, and then select **Delete model**. In this example, we've selected the model **OpportunityScoring_202009231810**.

    >[!NOTE]
    >You can't delete a model if the **Retrain automatically** option is enabled. Disable the option first.

    > [!div class="mx-imgBorder"]
    > ![Delete a model](media/si-admin-predictive-opportunity-scoring-delete-model.png "Delete a model")

3. In the confirmation message that appears, select **Delete**.

The model is deleted from your organization.

## Manage duplicate models

When you create multiple models, it’s important to configure them so that they are not scoring the same opportunities. In the case of duplicate models, it’s difficult to identify which model scored the opportunity. Duplicate models are automatically identified when you create or edit models. A conflict message is displayed when the configuration is a duplicate of existing models.    
To remove the duplicate models, use one of the following methods depending on the action you’re performing:
- [Create model](#create-model)
- [Published model](#published-model)

### Create model

When you select **Get started**, a conflict message is displayed stating that the model you’re creating has conflicts with other models in the list. The following image shows an example of the message:

> [!div class="mx-imgBorder"]
> ![A conflict message while creating the model.](media/si-admin-predictive-opportunity-scoring-duplicate-model-create-model-message.png "A conflict message while creating the model")
 
-	Select **No, Go back** and change the filter column and values to ensure that the model is scoring unique opportunities. For more information, see step 7 in [First-run setup experience](#first-run-setup-experience).
-Or-
-	Proceed to create a duplicate model, which might cause conflicts in identifying the opportunities that are being scored by this model.

### Published model

When you open a published model, a note displays above the version details section with the list of models that the current model is a duplicate of. The following image shows an example of the message:

> [!div class="mx-imgBorder"]
> ![A conflict message while publishing the model.](media/si-admin-predictive-opportunity-scoring-duplicate-model-published-model-message.png "A conflict message while publishing the model")
 
1.	Select **Delete model** to delete the current model from the application.
2.	[Create the model](#add-a-model) with different filter column and filter values to score a opportunity.

## Define entities for analytics

To display the list of business process flows that are defined for opportunities in your organization, and to allow for analytics, the business process flows entities must be enabled.

**To define entities for analytics**

1. Verify that **Change Tracking** is enabled for the business process flow entity for Azure Data Lake Storage. More information: [Enable change tracking to control data synchronization](/power-platform/admin/enable-change-tracking-control-data-synchronization)

2. Create an entry in `EntityAnalyticsConfig` to enable an entity for Data Lake Storage. Update the following columns:

    1. `ParentEntityLogicalName`: The logical name of the entity. 

    1. `IsEnabledForADLS`: If the value is **True**, the entity is enabled to sync to Data Lake Storage and if **False**, the entity won't sync to Data Lake Storage. By default, the value is **False**.

    > [!div class="mx-imgBorder"]
    > ![Create an entry for Data Lake Storage](media/si-admin-predictive-lead-scoring-create-entry-managed-lake.png "Create an entry for Data Lake Storage")

>[!NOTE]
>Change Tracking on the business process flow entity and `IsEnabledForADLS` must be configured as **True** to sync the data to Data Lake Storage using the Export to Data Lake service.

**Examples:**    

Create, read, update, and delete (CRUD) operations can be performed either through OData/SDK or solution import. 

- Sample to create an operation payload through OData:

    ```HTTP
    POST http://<OrgUrl>/api/data/v9.0/entityanalyticsconfigs
    {
        isenabledforadls: true,
        parententitylogicalname: "account"
    }
    ```

- Sample to patch an operation payload to update a record via OData:

    ```HTTP
    PATCH http://<OrgUrl>/api/data/v9.0/entityanalyticsconfigs(<copy guid from 'entityanalyticsconfigid' column>)
    {
        isenabledforadls: false
    }   
    ```    
    To learn more on how to use OData requests for Update and Delete, see [Update and delete entities using the Web API](/powerapps/developer/common-data-service/webapi/update-delete-entities-using-web-api)
     
- Sample to manage a solution to enable Account and Contact entities for Data Lake Storage. Create the following three XML files, and zip them into **ADLSConfigDataSampleTest.zip**.
    - **[Content_Types].xml**
        ```XML
        <?xml version="1.0" encoding="UTF-8"?>
        -<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
        <Default ContentType="application/octet-stream" Extension="xml"/>
        </Types>
        ```
    - **customizations.xml**
        ```XML
        <?xml version="1.0"?>
        -<ImportExportXml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Entities/>
        <Roles/>
        <Workflows/>
        <FieldSecurityProfiles/>
        <Templates/>
        <EntityMaps/>
        <EntityRelationships/>
        <OrganizationSettings/>
        <optionsets/>
        <CustomControls/>
        <EntityDataProviders/>
        -<EntityAnalyticsConfigs>
            -<EntityAnalyticsConfig>
                <parententitylogicalname>account</parententitylogicalname>
                <isenabledforadls>1</isenabledforadls>
            </EntityAnalyticsConfig>
            -<EntityAnalyticsConfig>
                <parententitylogicalname>contact</parententitylogicalname>
                <isenabledforadls>1</isenabledforadls>
            </EntityAnalyticsConfig>
        </EntityAnalyticsConfigs>
        -<Languages>
            <Language>1033</Language>
        </Languages>
        </ImportExportXml>
        ```
    - **solution.xml**
        ```XML
        <?xml version="1.0"?>
        -<ImportExportXml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" generatedBy="OnPremise" languagecode="1033" SolutionPackageVersion="9.1" version="9.1.0.5507">
        -<SolutionManifest>
            <UniqueName>ADLSConfigDataTest</UniqueName>
            -<LocalizedNames>
                <LocalizedName languagecode="1033" description="ADLSConfigDataTest"/>
            </LocalizedNames>
            <Descriptions/>
            <Version>1.0</Version>
            <Managed>1</Managed>
            -<Publisher>
                <UniqueName>Cr7e230</UniqueName>
                -<LocalizedNames>
                    <LocalizedName languagecode="1033" description="CDS Default Publisher"/>
                </LocalizedNames>
                <Descriptions/>
                <EMailAddress xsi:nil="true"/>
                <SupportingWebsiteUrl xsi:nil="true"/>
                <CustomizationPrefix>cr926</CustomizationPrefix>
                <CustomizationOptionValuePrefix>88399</CustomizationOptionValuePrefix>
                -<Addresses>
                    -<Address>
                        <AddressNumber>1</AddressNumber>
                        <AddressTypeCode xsi:nil="true"/>
                        <City xsi:nil="true"/>
                        <County xsi:nil="true"/>
                        <Country xsi:nil="true"/>
                        <Fax xsi:nil="true"/>
                        <FreightTermsCode xsi:nil="true"/>
                        <ImportSequenceNumber xsi:nil="true"/>
                        <Latitude xsi:nil="true"/>
                        <Line1 xsi:nil="true"/>
                        <Line2 xsi:nil="true"/>
                        <Line3 xsi:nil="true"/>
                        <Longitude xsi:nil="true"/>
                        <Name xsi:nil="true"/>
                        <PostalCode xsi:nil="true"/>
                        <PostOfficeBox xsi:nil="true"/>
                        <PrimaryContactName xsi:nil="true"/>
                        <ShippingMethodCode xsi:nil="true"/>
                        <StateOrProvince xsi:nil="true"/>
                        <Telephone1 xsi:nil="true"/>
                        <Telephone2 xsi:nil="true"/>
                        <Telephone3 xsi:nil="true"/>
                        <TimeZoneRuleVersionNumber xsi:nil="true"/>
                        <UPSZone xsi:nil="true"/>
                        <UTCOffset xsi:nil="true"/>
                        <UTCConversionTimeZoneCode xsi:nil="true"/>
                    </Address>
                    -<Address>
                        <AddressNumber>2</AddressNumber>
                        <AddressTypeCode xsi:nil="true"/>
                        <City xsi:nil="true"/>
                        <County xsi:nil="true"/>
                        <Country xsi:nil="true"/>
                        <Fax xsi:nil="true"/>
                        <FreightTermsCode xsi:nil="true"/>
                        <ImportSequenceNumber xsi:nil="true"/>
                        <Latitude xsi:nil="true"/>
                        <Line1 xsi:nil="true"/>
                        <Line2 xsi:nil="true"/>
                        <Line3 xsi:nil="true"/>
                        <Longitude xsi:nil="true"/>
                        <Name xsi:nil="true"/>
                        <PostalCode xsi:nil="true"/>
                        <PostOfficeBox xsi:nil="true"/>
                        <PrimaryContactName xsi:nil="true"/>
                        <ShippingMethodCode xsi:nil="true"/>
                        <StateOrProvince xsi:nil="true"/>
                        <Telephone1 xsi:nil="true"/>
                        <Telephone2 xsi:nil="true"/>
                        <Telephone3 xsi:nil="true"/>
                        <TimeZoneRuleVersionNumber xsi:nil="true"/>
                        <UPSZone xsi:nil="true"/>
                        <UTCOffset xsi:nil="true"/>
                        <UTCConversionTimeZoneCode xsi:nil="true"/>
                    </Address>
                </Addresses>
            </Publisher>
            -<RootComponents>
                <RootComponent behavior="0" schemaName="account" type="430"/>
                <RootComponent behavior="0" schemaName="contact" type="430"/>
            </RootComponents>
            <MissingDependencies/>
        </SolutionManifest>
        </ImportExportXml>
        ```



## Add the opportunity scoring widget to a form

By default, the predictive opportunity scoring widget is available only in the out-of-the-box **Sales Insights** form. If you're using customized forms for opportunities, you can display the predictive opportunity scoring widget on your custom forms by following these steps.

> [!IMPORTANT]
> - Adding opportunity scoring widgets is only supported in Unified Interface apps.
> - You can't use the legacy form designer to add an opportunity scoring widget to a form.

1. Sign in to the [Power Apps](https://make.powerapps.com/) portal.

    > [!div class="mx-imgBorder"]  
    > ![Power Apps home page](media/power-apps-home-page.png "Power Apps home page")

2. Search for and select your organization's environment.

    > [!div class="mx-imgBorder"]  
    > ![Select your organization](media/power-apps-select-org.png "Select your organization")

3. Select **Data** > **Tables**.

    The **Tables** page opens with the list of tables.

    > [!div class="mx-imgBorder"]  
    > ![Tables page with list of tables](media/power-apps-entities-page.png "Tables page with list of tables")

4. Open the table, select the **Forms** tab, and then select a main form to add the widget to. In this example, the table **Opportunity** is selected and the main form **Opportunity** is selected.

    >[!NOTE]
    >If you're unable to view the table to which you want to add the widget, in the upper-right corner of the page, change the filters settings to **All**. 

    > [!div class="mx-imgBorder"]  
    > ![Select the Opportunity main form underon the Forms tab](media/power-apps-opportunity-main-form.png "Select the Opportunity main form on the Forms tab")

5. In the form designer, select **Component**, and then from **Layout**, add a column to the form as a placeholder to add the widget.

    > [!div class="mx-imgBorder"]  
    > ![Add a column to the form](media/power-apps-layout-add-column-form.png "Add a column to the form")

    The column is added to the form.

7. From the site map, select **Display** > **Predictive score**.

    >[!NOTE]
    >Ensure that the added placeholder column is selected. If it isn't, the widget will be added at a random place in the form. 

    > [!div class="mx-imgBorder"]  
    > ![Select the predictive score widget](media/power-select-predictive-score-widget.png "Select the predictive score widget")

8. In the **Edit predictive score** pop-up window, select **Done**.

    > [!div class="mx-imgBorder"]  
    > ![Select Done to add the predictive score widget](media/power-app-predictive-score-widget-options.png "Select Done to add the predictive score widget")

    The predictive score widget is added to the form, as shown in the following image.

    > [!div class="mx-imgBorder"]  
    > ![Predictive score widget added to the form](media/power-app-predictive-opportunity-score-widget-added.png "Predictive score widget added to the form")

    >[!NOTE]
    >To hide the **New section** label, go to the **Properties** tab of the **New Section** settings pane, and then select **Hide label**.

9. Save and publish the form.

[!INCLUDE[cant-find-option](../includes/cant-find-option.md)]

### See also

[Prioritize opportunities through scores](../sales/work-predictive-opportunity-scoring.md)  
[Install and configure premium Sales Insights features](intro-admin-guide-sales-insights.md#install-and-configure-premium-sales-insights-features)

[!INCLUDE[footer-include](../includes/footer-banner.md)]
