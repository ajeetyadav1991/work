**Reminder for me**: Don't try to speak too much "tech language" - focus on business requirements and user pain points. Let engineers translate to technical solutions.

### 1. Business Context
"Our DPN Anomaly Detection tool currently works with sample data. We need to connect it to Google Cloud backend to handle real production data - potentially millions of anomaly records. 
We need your help planning this migration."

### 2. State User Requirements (5 minutes)
**Use this exact checklist**:

  Must Have (Critical):

    ✅ Secure access - only authorized users can view/edit
    ✅ Multiple users can classify anomalies simultaneously (server side)
    
  
  Should Have (Important):
  
    ✅ See real-time updates when colleagues classify anomalies
    ✅ Save work if internet disconnects temporarily ( cashing apply to browser side/ frontend)


### 3. Questions
**On Architecture**:

"Should we use pagination (20 rows per page) or infinite scroll? What's better for our use case?"
"How do we prevent two analysts from classifying the same anomaly at the same time?" - Race condition should be implemented by you or me.
"What happens if someone's internet drops while submitting a classification?" - Caching works ?
