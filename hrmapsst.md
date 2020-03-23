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
 * Fetch the employee details from the employee master table
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
 * Fetch the employee details from the below query.
  ```SQL
  EXEC SQL                                    
    SELECT MPL_EMPL_SSNO                    
          ,MPL_BN_EMPL_ID                   
          ,MPL_EMPL_LNME                    
          ,MPL_EMPL_FNME                    
          ,MPL_EMPL_MNME                    
          ,MPL_EMPL_GENDER                  
      INTO :MPL-EMPL-SSNO                   
          ,:MPL-BN-EMPL-ID                  
          ,:MPL-EMPL-LNME                   
          ,:MPL-EMPL-FNME                   
          ,:MPL-EMPL-MNME                   
          ,:MPL-EMPL-GENDER                 
      FROM CT.CEMPLOYEE                     
     WHERE MPL_EMPL_SSNO  = :MPL-EMPL-SSNO  
  END-EXEC
  ```
 * Fetch the employee details from the below query
  ```SQL
  EXEC SQL                                          
   SELECT DISTINCT                                
          POS_PAYROLL_NUMB                        
         ,POS_POS_NUMB                            
         ,POS_POS_TITLE                           
         ,POS_CITY_333                            
         ,POS_STATE                               
         ,POS_SNRTY_DIST_NBR                      
         ,POS_SUPV_SSN                            
         ,POS_POS_HQ_FLG                          
         ,POS_ASGN_WRK_STM                        
         ,POS_ASGN_WRK_ETM                        
         ,POS_BN_ORIG_RD_CD                       
         ,GNG_GANG_DESC                           
         ,GNG_CONSL_DIST                          
         ,GNG_GANG_ID                             
         ,GNG_SUPV_RPTG_ID                        
         ,MBP_COMMON_PT_CD                        
         ,MBP_MCH_ID_NBR          
         ,POS_POS_EFF_DT          
         ,POS_ADD_TMSTP           
         ,GNG_GANG_EFF_DT         
     INTO :POS-PAYROLL-NUMB       
         ,:POS-POS-NUMB           
         ,:POS-POS-TITLE          
         ,:POS-CITY-333           
         ,:POS-STATE              
         ,:POS-SNRTY-DIST-NBR     
         ,:POS-SUPV-SSN           
         ,:POS-POS-HQ-FLG         
         ,:POS-ASGN-WRK-STM       
         ,:POS-ASGN-WRK-ETM       
         ,:POS-BN-ORIG-RD-CD      
         ,:GNG-GANG-DESC          
         ,:GNG-CONSL-DIST         
         ,:GNG-GANG-ID            
         ,:GNG-SUPV-RPTG-ID                         
         ,:MBP-COMMON-PT-CD                         
         ,:MBP-MCH-ID-NBR                           
         ,:POS-POS-EFF-DT                           
         ,:POS-ADD-TMSTP                            
         ,:GNG-GANG-EFF-DT                          
    FROM CT.CNOP_POSITION A                         
        ,CT.CGANG B                                 
        ,CT.CMAPS_BLTN_POS C                        
   WHERE POS_POS_NUMB       = :MVC-POS-NBR          
     AND (POS_SNRTY_DIST_NBR = :MVC-SNRTY-DIST-NBR  
      OR  POS_CONSL_DIST     = :MVC-SNRTY-DIST-NBR)             
     AND POS_POS_EFF_DT    <= :RQT-DATE             
     AND (POS_TRM_DT       >= :RQT-DATE             
      OR POS_TRM_DT        IS NULL)                 
     AND POS_DEL_TMSTP     IS NULL  
     AND MBP_POS_NUMB       = POS_POS_NUMB        
     AND MBP_POS_EFF_DT     = POS_POS_EFF_DT      
     AND (MBP_TRM_DT       >= POS_POS_EFF_DT      
      OR MBP_TRM_DT        IS NULL)               
     AND MBP_DEL_TMSTP     IS NULL                
     AND GNG_GANG_ID        = POS_SUPV_SSN        
     AND GNG_GANG_EFF_DT   <= POS_POS_EFF_DT      
     AND (GNG_TRM_DT       >= POS_POS_EFF_DT      
      OR GNG_TRM_DT        IS NULL)               
     AND GNG_DEL_TMSTP     IS NULL                
  END-EXEC 
  ```
   * When Record  or multiple records found in above query, then set payroll number, position number, position title and state etc.
   
 * If position head quarter flag is equal to ‘1’ or ‘2’  and common point code is equal to ‘KM’ OR ‘KC’.
  ```SQL
  EXEC SQL                                             
    SELECT DISTINCT                                  
           MAS_FRMR_RD_CD                            
      INTO :MAS-FRMR-RD-CD                           
     FROM CT.CMAPS_SDIST_DESC                        
    WHERE MAS_SNRTY_DIST_NBR  = :POS-SNRTY-DIST-NBR  
      AND MAS_DEL_TMSTP      IS NULL                 
  END-EXEC
  ```
 * Fetch the assignment begin date and add timestamp from the below query.
  ```SQL
  EXEC SQL                                                   
    SELECT DISTINCT                                        
           CHAR(PAS_ASGN_BGN_DT, USA)                      
          ,PAS_ADD_TMSTP                                   
      INTO :PAS-ASGN-BGN-DT                                
          ,:PAS-ADD-TMSTP                                  
      FROM CT.CEMPL_POS_ASGN A                             
     WHERE PAS_PAYROLL_NUMB   = :POS-PAYROLL-NUMB          
       AND PAS_POS_NUMB       = :POS-POS-NUMB              
       AND PAS_EMPL_SSNO      = :MEM-EMPL-SSNO             
       AND PAS_SNRTY_DIST_NBR = :POS-SNRTY-DIST-NBR        
       AND PAS_ASGN_CHG_CD   IN ('CC','CX')                
       AND PAS_DEL_TMSTP     IS NULL                       
       AND PAS_ASGN_BGN_DT    =                            
           (SELECT MAX(PAS_ASGN_BGN_DT)                    
              FROM CT.CEMPL_POS_ASGN B                     
             WHERE PAS_PAYROLL_NUMB   = A.PAS_PAYROLL_NUMB 
               AND PAS_POS_NUMB       = A.PAS_POS_NUMB     
               AND PAS_EMPL_SSNO      = A.PAS_EMPL_SSNO      
               AND PAS_SNRTY_DIST_NBR = A.PAS_SNRTY_DIST_NBR 
               AND PAS_ASGN_CHG_CD   IN ('CC','CX')          
               AND PAS_DEL_TMSTP     IS NULL)                
  END-EXEC
  ```
 * If mechanical id number is not equal to empty, then fetch the mechanical description from the below query.
  ```SQL
  EXEC SQL                                      
    SELECT MMA_MCH_DESC                        
      INTO :MMA-MCH-DESC                       
      FROM CT.CMAPS_MACHINES                   
     WHERE MMA_MCH_ID_NBR    = :MBP-MCH-ID-NBR 
       AND MMA_DEL_TMSTP    IS NULL            
  END-EXEC
  ```
 * If gang supervisor rptg id is not equal to empty, then fetch the gang location details from the below query.
  ```SQL
  EXEC SQL                                        
    SELECT TMD_LINE_SEG                         
          ,TMD_STRT_ENGR_MP                     
          ,TMD_END_ENGR_MP                      
          ,TMD_BILL_TO_CTR                      
          ,CHAR(TMD_EFF_DT, USA)                
      INTO :BUMPC-LINE-SEG                      
          ,:STRT-MP                             
          ,:END-MP                              
          ,:BUMPC-BILL-TO-CTR                   
          ,:BUMPC-EFF-DT                        
      FROM CT.CEMPL_TIME_RPTD A                 
     WHERE TMD_GANG_ID         = :GNG-GANG-ID   
       AND TMD_POS_RLF_NBR    ¬= :HI-ONE        
       AND TMD_PAY_DTL_ADJ    ¬= 'R'            
       AND TMD_PAY_DTL_RVS    ¬= 'R'            
       AND TMD_CASH_ADJ_PD_CD ¬= 'Y'            
       AND TMD_DEL_TMSTP      IS NULL           
       AND TMD_BILL_TO_CTR    ¬= '99999'             
       AND TMD_STRT_ENGR_MP   ¬= 0.0                 
       AND TMD_EFF_DT         >= :GNG-GANG-EFF-DT    
     FETCH FIRST 1 ROW ONLY                          
  END-EXEC                                                                                
  ```
 * Fetch the supervisor position details using below query.
  ```SQL
  EXEC SQL                                      
   SELECT TSP_EXM_SUPV_POS_NBR_ID             
        , PERSON_ID                           
        , EMPL_FNME                           
        , EMPL_MNME                           
        , EMPL_LNME                           
        , SUPV_PERSON_ID                      
     INTO :TSP-EXM-SUPV-POS-NBR-ID            
        , :KW-PERSON-ID                       
        , :KW-EMPL-FNME                       
        , :KW-EMPL-MNME                       
        , :KW-EMPL-LNME                       
        , :KW-SUPV-PERSON-ID                  
     FROM CT.CGANG_TO_SUPV_POS                
         ,KW.TEMPL                            
    WHERE TSP_EFF_DT       <= :RQT-DATE       
      AND (TSP_TRM_DT      >= :RQT-DATE       
       OR TSP_TRM_DT       IS NULL)           
      AND TSP_GANG_ID       = :POS-SUPV-SSN        
      AND TSP_DEL_TS       IS NULL                 
      AND TSP_EXM_SUPV_POS_TYP_CD  = 'PERM'        
  
      AND SUBSTR(EMPL_POS_NBR,1,3) = '000'         
      AND SUBSTR(EMPL_POS_NBR,4,5) =               
          SUBSTR(TSP_EXM_SUPV_POS_NBR_ID,4,5)      
      AND EMPL_PERSID_ALT_CD  = 'N'                
  END-EXEC
  ```
   * When Record found in above query, then set the employee id, employee last name, employee first name and employee middle name.
   
 * If employee last name is equal to empty and supervisor RPTG id is not equal to empty, then fetch the supervisor details from the below query.
  ```SQL
  EXEC SQL                                                  
    SELECT SUP_EMPL_SSNO                                  
          ,MPL_EMPL_LNME                                  
          ,MPL_EMPL_FNME                                  
          ,MPL_EMPL_MNME                                  
      INTO :SUP-EMPL-SSNO                                 
          ,:MPL-EMPL-LNME                                 
          ,:MPL-EMPL-FNME                                 
          ,:MPL-EMPL-MNME                                 
      FROM CT.CSUPERVISOR A                               
          ,CT.CEMPLOYEE                                             
     WHERE SUP_SUPV_RPTG_ID  = :TSP-EXM-SUPV-POS-NBR-ID   
       AND SUP_DEL_TMSTP    IS NULL                       
       AND MPL_EMPL_SSNO     = A.SUP_EMPL_SSNO            
     FETCH FIRST 1 ROW ONLY                               
  END-EXEC                                                  
  ```
 * Fetch the employee details from the employee table
  ```SQL
  EXEC SQL                                    
    SELECT MPL_EMPL_SSNO                    
          ,MPL_BN_EMPL_ID                   
          ,MPL_EMPL_LNME                    
          ,MPL_EMPL_FNME                    
          ,MPL_EMPL_MNME                    
          ,MPL_EMAIL_ID                     
      INTO :MPL-EMPL-SSNO                   
          ,:MPL-BN-EMPL-ID                  
          ,:MPL-EMPL-LNME                   
          ,:MPL-EMPL-FNME                   
          ,:MPL-EMPL-MNME                   
          ,:MPL-EMAIL-ID                    
      FROM CT.CEMPLOYEE                     
     WHERE MPL_USER_ID    = :MPL-USER-ID    
  END-EXEC     
  ```
## FAX-POP-UP
 * Copy the Sub string of Request argument from 2nd character and length of 1 character to value. 
 * If value is equal to ‘R’, then fetch the report details from the below query.
  ```SQL
  EXEC SQL DECLARE RESEND2 CURSOR FOR            
    SELECT MRE_RPT_DVC_ID                      
          ,MRE_RPT_DVC_TYP_CD                  
      FROM CT.CMAPS_REPORT_REQ                 
     WHERE MRE_MAPS_RPT_ID = :MRE-MAPS-RPT-ID  
     ORDER BY MRE_RPT_DVC_TYP_CD               
             ,MRE_RPT_DVC_ID                   
  END-EXEC.                                      
  ```
   * Based on the device type code set the report device id.
   
 * If value is not equal to ‘R’, then fetch the application code description from the below query
