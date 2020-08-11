# Trigger-to-update-parent-records-field-when-child-record-is-updated
We are going to learn about How to update the parent record field based on child record trigger in Salesforce custom object.<br/><br/>
<a href="https://www.w3web.net/update-parent-object-from-child/">Go to more details about this article__  <b><i>www.w3web.net</i></b></a><br/><br/>

  trigger checkboxStatus on childObjTrigger__c (before Insert, before update, after insert, after update, before delete, after delete) {
    if(trigger.isBefore){
        //system.debug('I am inside before event');   
    }
    else if(trigger.isAfter){
        //system.debug('I am inside after event');
        
        if(trigger.isUpdate){        
            for(childObjTrigger__c myStatus: Trigger.new){      
                parentObjTrigger__c getParentObj = [SELECT Id, Status__c FROM parentObjTrigger__c WHERE Id = :myStatus.childLookup__c ];         
                childObjTrigger__c getChildObj = [SELECT Id, Status__c, childLookup__r.Status__c FROM childObjTrigger__c WHERE Id = :myStatus.Id];                  
                getParentObj.Status__c= getChildObj.Status__c;
                update getParentObj;         
            }
            
        }
        
        
        if(trigger.isInsert && (trigger.isAfter)){
            for(childObjTrigger__c myStatus: Trigger.new){
                parentObjTrigger__c getParentObj = [SELECT Id, Status__c FROM parentObjTrigger__c WHERE Id = :myStatus.childLookup__c ];         
                childObjTrigger__c getChildObj = [SELECT Id, Status__c, childLookup__r.Status__c FROM childObjTrigger__c WHERE Id = :myStatus.Id];
                getParentObj.Status__c= getChildObj.Status__c;        
                update getParentObj;
            }   
        }
        
        
        if(Trigger.isDelete && (Trigger.isBefore)){
            for(childObjTrigger__c myStatus: Trigger.old){          
                parentObjTrigger__c getParentObj = [SELECT Id, Status__c FROM parentObjTrigger__c WHERE Id = :myStatus.childLookup__c ];         
                childObjTrigger__c getChildObj = [SELECT Id, Status__c, childLookup__r.Status__c FROM childObjTrigger__c WHERE Id = :myStatus.Id];                           
                delete getParentObj;
            }   
        }   
        
    }
    
    
}
