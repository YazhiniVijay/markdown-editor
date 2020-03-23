# HRMAPSST 177-200

## GANG-LOCATION
 * Fetch the gang location details from the below query
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
  EXEC SQL                     
    SELECT MEM_EMPL_SSNO     
          ,MEM_EMPL_ADDR1    
          ,MEM_CITY_NME      
          ,MEM_STATE         
          ,MEM_ZIP           
          ,MEM_EMPL_LNME     
          ,MEM_EMPL_FNME     
          ,MEM_EMPL_MINIT    
          ,MEM_EMPL_ID       
      INTO :MEM-EMPL-SSNO    
          ,:MEM-EMPL-ADDR1   
          ,:MEM-CITY-NME     
          ,:MEM-STATE        
          ,:MEM-ZIP          
          ,:MEM-EMPL-LNME    
          ,:MEM-EMPL-FNME    
          ,:MEM-EMPL-MINIT   
          ,:MEM-EMPL-ID                      
      FROM CT.CMAPS_EMPL_MASTER              
     WHERE MEM_EMPL_ID    = :MEM-EMPL-ID     
       AND MEM_DEL_TMSTP IS NULL             
END-EXEC          
  ```
 * Copy the Sub string of Request argument from 11th character and length of 26 characters to timestamp.
 * If timestamp is not equal to empty, then fetch the details from the below query.
 ```SQL
 EXEC SQL                                         
    SELECT DISTINCT                              
           MVC_CALL_TMSTP                        
          ,MVC_POS_NBR                           
          ,MVC_SNRTY_DIST_NBR                    
          ,MVC_ADD_USER_ID                       
          ,MVC_VACY_TYP_CD                       
      INTO :MVC-CALL-TMSTP                       
          ,:MVC-POS-NBR                          
          ,:MVC-SNRTY-DIST-NBR                   
          ,:MVC-ADD-USER-ID                      
          ,:MVC-VACY-TYP-CD                      
      FROM CT.CMAPS_VACY_CALLS                   
     WHERE MVC_EMPL_SSNO      = :MEM-EMPL-SSNO   
       AND MVC_CALL_TMSTP     = :MVC-CALL-TMSTP  
       AND MVC_DEL_TMSTP    IS NULL              
END-EXEC                                         
 ```
  * If timestamp is equal to empty, then fetch the details from the below query.
  ```SQL
  EXEC SQL                                        
    SELECT DISTINCT                             
           MVC_CALL_TMSTP                       
          ,MVC_POS_NBR                          
          ,MVC_SNRTY_DIST_NBR                   
          ,MVC_ADD_USER_ID                      
          ,MVC_VACY_TYP_CD                      
          ,CHAR(MVC_RCALL_LTR_DT, USA)          
      INTO :MVC-CALL-TMSTP                      
          ,:MVC-POS-NBR                         
          ,:MVC-SNRTY-DIST-NBR                  
          ,:MVC-ADD-USER-ID                     
          ,:MVC-VACY-TYP-CD                     
          ,:MVC-RCALL-LTR-DT                    
      FROM CT.CMAPS_VACY_CALLS A                
     WHERE MVC_EMPL_SSNO      = :MEM-EMPL-SSNO  
       AND MVC_POS_NBR        = :MVC-POS-NBR    
       AND MVC_DEL_TMSTP    IS NULL             
       AND MVC_CALL_TMSTP     =                      
           (SELECT MAX(MVC_CALL_TMSTP)               
              FROM CT.CMAPS_VACY_CALLS B             
             WHERE MVC_EMPL_SSNO   = A.MVC_EMPL_SSNO 
               AND MVC_POS_NBR     = A.MVC_POS_NBR   
               AND MVC_DEL_TMSTP  IS NULL)           
END-EXEC               
```
  
