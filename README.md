trigger leadTrigger on Lead (after insert, after update) 
{
    
    if(trigger.isAfter)
    {
        if(trigger.isUpdate)
        {
            leadTriggerHandler.createTaskAfterUpdate(Trigger.new, Trigger.oldMap);
        }
        if(trigger.isInsert)
        {
            leadTriggerHandler.createTaskAfterInsert(Trigger.new);
        }
        
    }
	
}
public with sharing class leadTriggerHandler 
{
    public static void createTaskAfterUpdate(List<Lead> newleads, Map<ID, Lead> oldleads)
    {
        List<Task> taskList = new List<Task>();
            	for(Lead l : newleads)
                {
                    if(l.ProductInterest__c != oldLeads.get(l.Id).ProductInterest__c)
                    {
                        Task task = new Task();
            			task.Status = l.Status;
            			task.WhoId = l.Id;
            			task.Subject = 'new Task after change in Product Interest';
                        task.Priority = 'Normal';
            			taskList.add(task);
                    
                    }
                }
        insert taskList;
     
    }
    
    public static void createTaskAfterInsert(List<Lead> leadList)
    {
        List<Task> taskList2 = new List<Task>();
        for(Lead ld : leadList)
        {
            Task task = new Task();
            task.Status = ld.Status;
            task.WhoId = ld.Id;
            task.Subject = 'new Task';
            taskList2.add(task);
        }
        insert taskList2;
    }
	
}
