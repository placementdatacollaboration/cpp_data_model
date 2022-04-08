# CPP Data Model
Model to track the bidding and placement process for children in care 

# Current Structure:
Here is an ER Diagram showing the current relationships between the different tables.  
For a breakdown of the different tables, please see the linked [YAML file](data_model.yaml).

Because of the complexity of this model, I've tried to group things into sections.

## Child
```mermaid
erDiagram
    Child ||--o{ ChildNote : "has a"
    Child }|--o{ ChildSiblingGroup : "is part of a"
    Child ||--|{ ChildExternalReference : "has a"
    Child ||..o{ ChildNeed: "has a"
    ChildNeed }|..|| Need: "is defined by"
    Child ||--|| ChildSupportServiceBridge: "is connected by"
```

## ResidentialBid
```mermaid
erDiagram
    ChildResidentialBidBridge ||--|| ResidentialBid : "is connected by"
    ChildResidentialBidBridge ||--|| Child : "is connected to"
    ResidentialPlacementBidResponse ||--|| ResidentialBidResponse : "has a"
    ResidentialPlacementBidResponse ||--|| ResidentialBid : "is part of"
    ResidentialPlacementBidResponse }|--|| ResidentialHome : "is enabled by"
    ResidentialPlacementBid }|--|| Child : "Is part of"
```    

## ResidentialPlacement
```mermaid
erDiagram
    ResidentialPlacement ||--o{ ResidentialPlacementRevision : "is changed by" 
    ResidentialPlacement ||--o{ ResidentialPlacementNote : "has a"
    ResidentialPlacement ||--|| SupportServiceBid : "came from"
    ResidentialPlacement ||--|| ResidentialBid : "came from"
    ResidentialPlacement }|--|| ResidentialHome : "is in"
    ResidentialPlacement }|--|| Framework : "utilises"
    ResidentialPlacement ||--|| LocalAuthority : "placement"
    ResidentialPlacement ||--|| LocalAuthority : "host"
```

## Support Service
```mermaid
erDiagram
    SupportService ||--o{ SupportServiceNote : "has additional"
    SupportService ||--|{ SupportServicePlacement : "utilises"
    SupportServicePlacement ||--|| SupportServiceBid : "was implemented from"
    SupportServicePlacement ||--|| Framework : "utilises"
    SupportServicePlacement ||--|| LocalAuthority : "placement"
    SupportServicePlacement ||--o{ SupportServicePlacementRevision : "is refined by"    
```    

## Support Service Bid

```mermaid
erDiagram
    SupportServiceBid ||--|| ChildSupportServiceBridge: "connected by"
    SupportServiceBid ||--|| ServicePurchaser: "is handled by"
    SupportServiceBid ||--|{ ChildSupportServiceBridge : "connected by"
    ChildSupportServiceBid ||--|| SupportServiceBid: "is part of"
    Child ||--|| ChildSupportServiceBid: "is part of"
    SupportServiceBid }|--|| SupportServiceBidResponse : "has"
    FosterAgency ||--|{ SupportServiceBidResponse : "responded with"
```    
    
## Fostering Placement
```mermaid
erDiagram  
    FosteringPlacement ||--|| FosteringBid : "came from"
    FosteringPlacement }|--|| FosteringAgency : "is handled by"
    FosteringPlacement }|--|| FrameworkId : "utilises"
    FosteringPlacement }|--|| PlacementLocalAuthority : "is part of"
    FosteringPlacement ||--o{ FosteringPlacementRevision : "is refined by"
    FosteringPlacement ||--o{ FosteringPlacementNote : "has"
    FosteringAgency ||--o{ FosteringAgencyNote : "has"
```

## Fostering Placement Bid
```mermaid
erDiagram
    ServicePurchaser ||--|{ FosteringBid : "issued by"
    ChildFosteringBidBridge ||--|| Child : "is connected by"
    ChildFosteringBidBridge ||--|| FosteringBid : "is connected by"
```

## Provider
```mermaid
erDiagram
    Provider ||--|{ Home  : maintains
    Home ||--|{ HomeNote : has
    Provider ||--|{ ProviderNote : has
```