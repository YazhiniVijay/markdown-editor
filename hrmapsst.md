# HRMAPSST 177-200
## GANG-LOCATION
 * Fetch the gang location details from the below query.
  ```SQL
  EXEC SQL                                       
    SELECT DISTINCT                            
           GNG_GANG_HQ_CD                      
          ,GNG_FAX_ACD                         
          ,GNG_FAX_PFX                         
          ,GNG_FAX_NBR                         
          ,GNG_GANG_HQ_333                     
          ,GNG_GANG_HQ_ST                      
      INTO :GNG-GANG-HQ-CD                     
          ,:GNG-FAX-ACD                        
          ,:GNG-FAX-PFX                        
          ,:GNG-FAX-NBR                        
          ,:GNG-GANG-HQ-333                    
          ,:GNG-GANG-HQ-ST                     
      FROM CT.CGANG                            
     WHERE GNG_GANG_ID       = :GNG-GANG-ID    
       AND GNG_CONSL_DIST    = :GNG-CONSL-DIST 
       AND GNG_GANG_EFF_DT  <= :RQT-DATE       
       AND (GNG_TRM_DT      >= :RQT-DATE  
        OR GNG_TRM_DT       IS NULL)      
       AND GNG_DEL_TMSTP    IS NULL       
END-EXEC                                  
  ```
## RECALL-LETTER
 * Copy the Sub string of Request argument from 2nd character and length of 9 characters to employee id.
 * Fetch the employee details from the employee master table. 
 ```SQL
 exe sql
```

