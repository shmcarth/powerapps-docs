---
title: "Customize entity metadata (Common Data Service for Apps) | Microsoft Docs" # Intent and product brand in a unique string of 43-59 chars including spaces
description: "Entities are defined by metadata. By defining or changing the entity metadata, you can control the capabilities of an entity." # 115-145 characters including spaces. This abstract displays in the search result.
ms.custom: ""
ms.date: 10/31/2018
ms.reviewer: ""
ms.service: "powerapps"
ms.topic: "article"
author: "mayadumesh" # GitHub ID
ms.author: "jdaly" # MSFT alias of Microsoft employees only
manager: "ryjones" # MSFT alias of manager or PM counterpart
---
# Customize entity metadata

Entities are defined by metadata. By defining or changing the entity metadata, you can control the capabilities of an entity. To view the metadata for your organization, use the metadata browser. [Download the metadata browser](http://download.microsoft.com/download/8/E/3/8E3279FE-7915-48FE-A68B-ACAFB86DA69C/MetadataBrowser_3_0_0_5_managed.zip).

More information: [Browse the Metadata for Your Organization](browse-your-metadata.md)  
  
 This topic is about how to work with entities programmatically. See [Create or edit entities (record types)](/dynamics365/customer-engagement/customize/create-edit-entities)  for information about working with entities in the application.  

Entities can be created using either the Organization service or the Web API. The following information can be applied to both.

- With the Organization service you will use the <xref:Microsoft.Xrm.Sdk.Metadata.EntityMetadata> class. More information: [Create a custom entity](/dynamics365/customer-engagement/developer/org-service/create-custom-entity) and [Retrieve, update, and delete entities](/dynamics365/customer-engagement/developer/org-service/retrieve-update-delete-entities)
- With the Web API you will use the <xref:Microsoft.Dynamics.CRM.EntityMetadata> EntityType. More information : [Create and update entity definitions using the Web API](/dynamics365/customer-engagement/developer/webapi/create-update-entity-definitions-using-web-api).
 
<a name="BKMK_EntityMetadataMessages"></a>

## Entity metadata operations
How you work with entity metadata depends on which service you use.

Since the Web API is a RESTful endpoint, it uses a different way to create, retrieve, update, and delete metadata. Use `POST`, `GET`, `PUT`, and `DELETE` HTTP verbs to work with metadata entitytypes. More information : [Create and update entity definitions using the Web API](/dynamics365/customer-engagement/developer/webapi/create-update-entity-definitions-using-web-api).

One exception to this is the <xref href="Microsoft.Dynamics.CRM.RetrieveMetadataChanges?text=RetrieveMetadataChanges Function" /> provides a way to compose a metadata query and track changes over time. 

If working with Organization Service, use <xref:Microsoft.Xrm.Sdk.Messages.RetrieveMetadataChangesRequest> class. This class contains the data that is needed to to retrieve a collection of metadata records that satisfy the specified criteria. The <xref:Microsoft.Xrm.Sdk.Messages.RetrieveMetadataChangesResponse> returns a timestamp value that can be used with this request at a later time to return information about how metadata has changed since the last request.
   

|                                                                                                                                                                          Message                                                                                                                                                                           |                                               Web API                                                |                           SDK Assembly                           |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|                                                                                                                                                                        CreateEntity                                                                                                                                                                        |                         Use a POST request to send data to create an entity.                         |      <xref:Microsoft.Xrm.Sdk.Messages.CreateEntityRequest>       |
|                                                                                                                                                                        DeleteEntity                                                                                                                                                                        |                              Use a DELETE request to delete an entity.                               |      <xref:Microsoft.Xrm.Sdk.Messages.DeleteEntityRequest>       |
|                                                                                                                                                                    RetrieveAllEntities                                                                                                                                                                     |                               Use GET request to retrieve entity data.                               |   <xref:Microsoft.Xrm.Sdk.Messages.RetrieveAllEntitiesRequest>   |
|                                                                                                                                                                       RetrieveEntity                                                                                                                                                                       |          <xref href="Microsoft.Dynamics.CRM.RetrieveEntity?text=RetrieveEntity Function" />          |     <xref:Microsoft.Xrm.Sdk.Messages.RetrieveEntityRequest>      |
|                                                                                                                                                                        UpdateEntity                                                                                                                                                                        |                                Use a PUT request to update an entity.                                |      <xref:Microsoft.Xrm.Sdk.Messages.UpdateEntityRequest>       |
| RetrieveMetadataChanges</br>Used together with objects in the <xref:Microsoft.Xrm.Sdk.Metadata.Query> namespace to create a query to efficiently retrieve and detect changes to specific metadata. More information: [Retrieve and Detect Changes to Metadata](/dynamics365/customer-engagement/developer/retrieve-detect-changes-metadata). | <xref href="Microsoft.Dynamics.CRM.RetrieveMetadataChanges?text=RetrieveMetadataChanges Function" /> | <xref:Microsoft.Xrm.Sdk.Messages.RetrieveMetadataChangesRequest> |

<a name="BKMK_CreationOptions"></a>   
## Options available when you create a custom entity  
 The following table lists the options that are available when you create a custom entity. You can only set these properties when you create a custom entity.  

|Option|Description|  
|------------|-----------------|  
|**Create as custom activity**|You can create an entity that is an activity by setting the `IsActivity` property when using the organization service or Web API respectively. For more information, see [Custom Activities in Dynamics 365](custom-activities.md).|  
|**Entity Names**|There are two types of names, and both must have a customization prefix:<br /><br /> `LogicalName`: Name that is the version of the entity name that is set in all lowercase letters.<br /><br /> `SchemaName`: Name that will be used to create the database tables for the entity. This name can be mixed case. The casing that you use sets the name of the object generated for programming with strong types or when you use the REST endpoint.<br /><br /> **Note**: If the logical name differs from the schema name, the schema name will override the value that you set for the logical name.<br /><br /> When an entity is created in the application in the context of a specific solution, the customization prefix used is the one set for the `Publisher` of the solution. When an entity is created programmatically, you can set the customization prefix to a string that is between two and eight characters in length, all alphanumeric characters and it must start with a letter. It cannot start with “mscrm”. The best practice is to use the customization prefix defined by the publisher that the solution is associated with, but this is not a requirement. An underscore character must be included between the customization prefix and the logical or schema name.|  
|**Ownership**|Use the `OwnershipType` property to set this. Use the <xref:Microsoft.Xrm.Sdk.Metadata.OwnershipTypes> enumeration or <xref:Microsoft.Dynamics.CRM.OwnershipTypes> EnumType to set the type of entity ownership. The only valid values for custom entities are `OrgOwned` or `UserOwned`. For more information, see [Entity Ownership](/dynamics365/customer-engagement/developer/introduction-entities#EntityOwnership).|  
|**Primary Attribute**|With the Organization service, use <xref:Microsoft.Xrm.Sdk.Messages.CreateEntityRequest>.<xref:Microsoft.Xrm.Sdk.Messages.CreateEntityRequest.PrimaryAttribute> property to set this.<br /><br />With the Web API the JSON defining the entity must include one <xref:Microsoft.Dynamics.CRM.StringAttributeMetadata> with the `IsPrimaryName` property set to true.<br /><br /> In both cases string attribute must be formatted as `Text`. The value of this attribute is what is shown in a lookup for any related entities. Therefore, the value of the field should represent a name for the entity record.|  

<a name="BKMK_OptInOptions"></a>

## Enable entity capabilities  
 The following table lists entity capabilities. You can set these capabilities when you create an entity or you can enable them later. Once enabled, these capabilities cannot be disabled.  

|Capability|Description|  
|----------------|-----------------|  
|**Business Process flows**|Set `IsBusinessProcessEnabled` to true in order to enable the entity for business process flows.|  
|**Notes**| To create an entity relationship with the `Annotation` entity and enable the inclusion of a **Notes** area in the entity form. By including **Notes**, you can also add attachments to records. <br /><br />With the Organization service, use the <xref:Microsoft.Xrm.Sdk.Messages.CreateEntityRequest> or <xref:Microsoft.Xrm.Sdk.Messages.UpdateEntityRequest> `HasNotes` property <br /><br />With the Web API set the <xref:Microsoft.Dynamics.CRM.EntityMetadata>.`HasNotes` property.|  
|**Activities**|To create an entity relationship with the `ActivityPointer` entity so that all the activity type entities can be associated with this entity.<br /><br /> With the Organization service  use the <xref:Microsoft.Xrm.Sdk.Messages.CreateEntityRequest> or <xref:Microsoft.Xrm.Sdk.Messages.UpdateEntityRequest> `HasActivities` property. <br /><br /> With the Web API, set the  <xref:Microsoft.Dynamics.CRM.EntityMetadata>.`HasActivities` property.| 
|**Connections**|To enable creating connection records to associate this entity with other connection entities set the `IsConnectionsEnabled.Value` property value to true.|  
|**Queues**|Use the `IsValidForQueue` property to add support for queues. When you enable this option, you can also set the `AutoRouteToOwnerQueue` property to automatically move records to the owner’s default queue when a record of this type is created or assigned.|  
|**E-mail**|Set the `IsActivityParty` property so that you can send e-mail to an e-mail address in this type of record.|  

<a name="BKMK_OpenOptions"></a>   

## Editable entity properties  
 The following table lists entity properties that you can edit. Unless a managed property disallows these options, you can update them at any time.  


|                                        Property                                        |                                                                                                                                                                                                     Description                                                                                                                                                                                                     |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                 **Allow Quick Create**                                 |                                                                                   Use `IsQuickCreateEnabled` to enable quick create forms for the entity. Before you can use quick create forms you must first create and publish a quick create form.<br /> **Note**:<br /> Activity entities do not support quick create forms.                                                                                   |
|                                    **Access Teams**                                    |                                                                                                                                Use `AutoCreateAccessTeams` to enable the entity for access teams. See [About team templates](/dynamics365/customer-engagement/admin/about-team-templates) for more information.                                                                                                                                |
|                                   **Primary Image**                                    |                                                                                             If an entity has an image attribute you can enable or disable displaying that image in the application using `PrimaryImageAttribute`. For more information see [Entity Images](/dynamics365/customer-engagement/developer/introduction-entities#BKMK_EntityImages).                                                                                             |
|                                **Change display text**                                 |                                                                                             The managed property `IsRenameable` prevents the display name from being changed in the application. You can still programmatically change the labels by updating the `DisplayName` and `DisplayCollectionName` properties.                                                                                             |
|                            **Edit the entity Description**                             |                                                                                                         The managed property `IsRenameable` prevents the entity description from being changed in the application. You can still programmatically change the labels by updating the `Description` property.                                                                                                         |
|                            **Enable for use while offline**                            |                                                                                                          Use `IsAvailableOffline` to enable or disable the ability of Dynamics 365 for Microsoft Office Outlook with Offline Access users to take data for this entity offline.                                                                                                           |
|                          **Enable the Outlook Reading Pane**                           | **Note**:<br /><br /> The `IsReadingPaneEnabled` property is for internal use only.<br /><br /> To enable or disable the ability of Office Outlook users to view data for this entity, use the Outlook reading pane. You must set this property in the application. |
|                                 **Enable Mail Merge**                                  |                                                                                                                 Use `IsMailMergeEnabled` to enable or disable the ability to generate Office Word merged documents that use data from this entity.                                                                                                                  |
|                             **Enable Duplicate Detection**                             |                                                                                                       Use `IsDuplicateDetectionEnabled` to enable or disable duplicate detection for the entity. For more information, see [Detect Duplicate Data in Dynamics 365](/dynamics365/customer-engagement/developer/detect-duplicate-data-for-developers).                                                                                                        |
|                           **Enable SharePoint Integration**                            |                                                          Use `IsDocumentManagementEnabled` to enable or disable SharePoint server integration for the entity. For more information, see [Enable Document Management for Entities](/dynamics365/customer-engagement/developer/integration-dev/enable-document-management-entities).                                                          |
| **Enable Dynamics 365 for phones** |                                                                                                                      Use `IsVisibleInMobile` to enable or disable the ability of Dynamics 365 for phones users to see data for this entity.                                                                                                                       |
|              **Dynamics 365 for tablets**               |                             Use `IsVisibleInMobileClient` to enable or disable the ability of Dynamics 365 for tablets users to see data for this entity.<br /><br /> If the entity is available for Dynamics 365 for tablets you can use `IsReadOnlyInMobileClient` to specify that the data for the record is read-only.                              |
|                                  **Enable Auditing**                                   |                                                                                                              Use `IsAuditEnabled` to enable or disable auditing for the entity. For more information, see [Configure Entities and Attributes for Auditing](configure-entities-attributes-auditing.md).                                                                                                              |
|                        **Change areas that display the entity**                        |                                                                                                                                                  You can control where entity grids appear in the application Navigation Pane. This is controlled by the SiteMap.                                                                                                                                                   |
|                              **Add or Remove Attributes**                              |                                                                                     As long as the managed property `CanCreateAttributes.Value` allows for creating attributes, you can add attributes to the entity. For more information, see [Customize Entity Attribute Metadata](/dynamics365/customer-engagement/developer/customize-entity-attribute-metadata).                                                                                      |
|                                **Add or Remove Views**                                 |                                                                                                                                As long as the managed property `CanCreateViews.Value` allows for creating views, you can use the `SavedQuery` entity to create views for an entity.                                                                                                                                 |
|                                **Add or Remove Charts**                                |              As long as the managed property `CanCreateCharts.Value` allows for creating charts and the `IsEnabledForCharts` entity property is true, you can use the [SavedQueryVisualization Entity](/reference/entities/savedqueryvisualization.md) to create charts for an entity. For more information, see [View Data with Visualizations (Charts)](/dynamics365/customer-engagement/developer/customize-dev/view-data-with-visualizations-charts).              |
|                         **Add or Remove Entity Relationships**                         |                                                                                        There are several managed properties that control the types of entity relationships that you can create for an entity. For more information, see [Customize Entity Relationship Metadata](/dynamics365/customer-engagement/developer/customize-entity-relationship-metadata).                                                                                        |
|                                    **Change Icons**                                    |                                                                                                                                             You can change the icons used for custom entities. For more information, see [Modify Entity Icons](/dynamics365/customer-engagement/developer/modify-icons-entity).                                                                                                                                             |
|                        **Can Change Hierarchical Relationship**                        |                                                                                        `CanChangeHierarchicalRelationship.Value` controls whether the hierarchical state of entity relationships included in your managed solutions can be changed. More information:                                                                                         |
  
<a name="BKMK_MessagesForCustomEntites"></a>
 
## Messages supported by custom entities  
 Custom entities support the same base messages as system entities. The set of messages available depends on whether the custom entity is user-owned or organization owned. For more information, see [Actions on Entity Records](/dynamics365/customer-engagement/developer/introduction-entities#ActionsOnEntityRecords).  
  
### See also  
[Use the Web API with metadata](webapi/use-web-api-metadata.md)

[Work with metadata using the Organization service](org-service/work-with-metadata.md)