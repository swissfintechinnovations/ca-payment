Remaining differences to SIX bLink

**account API:**
-  /accounts/{accountId}/transactions: dateFrom, dateTo, bookingStatus, cursor, and limit <-> date
-  missing /consents
-  missing /healthcheck: PR in progress



**general:**
not used ref:


PortfolioListLink:
      type: object
      title: Partner Link
      properties:
      
        rel:
          type: string
        
        href:
          type: string
          example: /mortgage-app/api/v1/mortgages
          description: |
                    The machine or tenant relative URI that points to the linked resource
        
                    Example: 
                    Assuming that the actual URL of the resource would be: https://machine/tenant/app/api/v1/detail-view/detailId,
                    then the href to the detail-view would be: /app/api/v1/detail-view/detailId
        
        version:
          type: string
          example: v1
          description: Version of the API (the same rel can appear several times for different versions that are supported)
        
        method:
          $ref: "#/components/schemas/HttpVerb"
        
        type:
          type: string
          example: "application/json"
          description: The supported http content-type when sending the request to the server. If no content is supported on an end-point (like on a typical GET) then the type will be null


HttpVerb:
    type: string
    example: GET
    enum:
    - GET
    - POST
    - PUT
    - PATCH