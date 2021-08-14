# utl-frequencies-and-distinct-counts-by-one-or-two-dimension-variables
Frequencies and distinct counts by one or two dimension variables

    %let pgm=utl-frequencies-and-distinct-counts-by-one-or-two-dimension-variables;                                                              
                                                                                                                                                 
    Frequencies and distinct counts by one or two dimension variables                                                                            
                                                                                                                                                 
    Macros on the end of this post                                                                                                               
                                                                                                                                                 
    Also at                                                                                                                                      
                                                                                                                                                 
    GitHub                                                                                                                                       
    https://tinyurl.com/daunke3r                                                                                                                 
    https://github.com/rogerjdeangelis/utl-frequencies-and-distinct-counts-by-one-or-two-dimension-variables                                     
                                                                                                                                                 
    Performance Package 100s of tools?                                                                                                           
    https://tinyurl.com/jek5z8kw                                                                                                                 
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                                   
                                                                                                                                                 
    This post is set up for Edit Checking FDA SDTMs. The frequencies and distict counts are for                                                  
    variable usubjid. Which is common to almost all FDA SDTMs.                                                                                   
                                                                                                                                                 
    FYI: GitHub is a development site. Although I try to make sure my code is bullet proof,                                                      
    I cannot guaranetee it. I really appreciate the feedback I get when the code breaks,                                                         
           _     _           _   _                                                                                                               
      ___ | |__ (_) ___  ___| |_(_)_   _____                                                                                                     
     / _ \| `_ \| |/ _ \/ __| __| \ \ / / _ \                                                                                                    
    | (_) | |_) | |  __/ (__| |_| |\ V /  __/                                                                                                    
     \___/|_.__// |\___|\___|\__|_| \_/ \___|                                                                                                    
              |__/                                                                                                                               
                                                                                                                                                 
    Add Unique counts for usubjid to proc freq output                                                                                            
                                                                                                                                                 
    Dataset=pain_study  frequecies and distinct count for trt race (hardcoded for usubjid)                                                       
                                                                                                                                                 
                                       UNIQUE_  PROC FREQ                                                                                        
    TRT               RACE             USUBJID  OBSEVATIONS                                                                                      
    -------------------------------------------------------                                                                                      
    ASPIRIN           MARTIAN               15           18                                                                                      
    ASPIRIN           OTHER                  4            5                                                                                      
    ASPIRIN           VENUSIAN              12           13                                                                                      
    IBUPROEN          MARTIAN                4            5                                                                                      
    IBUPROEN          OTHER                  1            1                                                                                      
    Placebo           MARTIAN                4            4                                                                                      
    Placebo           OTHER                  2            2                                                                                      
    Placebo           VENUSIAN               2            2                                                                                      
     _                   _                                                                                                                       
    (_)_ __  _ __  _   _| |_                                                                                                                     
    | | `_ \| `_ \| | | | __|                                                                                                                    
    | | | | | |_) | |_| | |_                                                                                                                     
    |_|_| |_| .__/ \__,_|\__|                                                                                                                    
            |_|                                                                                                                                  
                                                                                                                                                 
    data pain_study ;                                                                                                                            
      length trt $16 race $24;                                                                                                                   
      set sashelp.cars(rename=(make=race drivetrain=trt)  keep= make drivetrain where=(race=:"C"));                                              
      usubjid = int(31*uniform(1234));                                                                                                           
      select (race);                                                                                                                             
        when ('Cadillac '  ) race='OTHER';                                                                                                       
        when ('Chevrolet'  ) race='MARTIAN';                                                                                                     
        when ('Chrysler '  ) race='VENUSIAN';                                                                                                    
      end; /* no need for otherwise */                                                                                                           
      select (trt);                                                                                                                              
        when ('All'  ) trt='IBUPROEN';                                                                                                           
        when ('Front') trt='ASPIRIN';                                                                                                            
        when ('Rear' ) trt='Placebo';                                                                                                            
      end;                                                                                                                                       
      pain_level=int(11*uniform(2345));                                                                                                          
    run;quit;                                                                                                                                    
                                                     Performance Macros                                                                          
                                                                                                                                                 
    Up to 40 obs WORK.PAIN_STUDY total obs=50        ctl left mouse button made this)                                                            
                                              PAIN_                                                                                              
    Obs      TRT       RACE        USUBJID    LEVEL                                                                                              
                                                                                                                                                 
      1    ASPIRIN     OTHER           7         0                                                                                               
      2    ASPIRIN     OTHER          11         1                                                                                               
      3    Placebo     OTHER           7         0                                                                                               
      4    ASPIRIN     OTHER           1         1                                                                                               
      5    ASPIRIN     OTHER          13         1                                                                                               
      6    ASPIRIN     OTHER           1         5                                                                                               
      7    Placebo     OTHER           2         9                                                                                               
      8    IBUPROEN    OTHER          29         8                                                                                               
      9    ASPIRIN     MARTIAN        12         4                                                                                               
     10    IBUPROEN    MARTIAN        10         5                                                                                               
     11    ASPIRIN     MARTIAN         8         3                                                                                               
                                                                                                                                                 
    last 11 obs WORK.PAIN_STUDY total obs=50                                                                                                     
                                                                                                                                                 
                                             PAIN_    tail on command line produced this                                                         
    Obs      TRT        RACE      USUBJID    LEVEL                                                                                               
                                                                                                                                                 
     40    ASPIRIN    VENUSIAN        9         3                                                                                                
     41    ASPIRIN    VENUSIAN        9         4                                                                                                
     42    ASPIRIN    VENUSIAN       17        10                                                                                                
     43    ASPIRIN    VENUSIAN        3         9                                                                                                
     44    ASPIRIN    VENUSIAN        1         0                                                                                                
     45    ASPIRIN    VENUSIAN       24        10                                                                                                
     46    ASPIRIN    VENUSIAN       26         4                                                                                                
     47    ASPIRIN    VENUSIAN        0        10                                                                                                
     48    ASPIRIN    VENUSIAN       30         0                                                                                                
     49    Placebo    VENUSIAN       30         9                                                                                                
     50    Placebo    VENUSIAN        5         9                                                                                                
                                                                                                                                                 
                                                                                                                                                 
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                           
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|                                                                                                          
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                          
    | .__/|_|  \___/ \___\___||___/___/                                                                                                          
    |_|                                                                                                                                          
     _   _ _ __   __ _                                                                                                                           
    | | | | `_ \ / _` |                                                                                                                          
    | |_| | | | | (_| |                                                                                                                          
     \__,_|_| |_|\__, |                                                                                                                          
                    |_|                                                                                                                          
                                                                                                                                                 
    If you want this outputs for the last created sas dataset then                                                                               
                                                                                                                                                 
    Type 'unq trt' and this will appear in output window                                                                                         
                                                                                                                                                 
    Dataset=_last_  frequecies and distinct count for trt                                                                                        
                           UNIQUE_                                                                                                               
    TRT                    USUBJID  OBSEVATIONS                                                                                                  
    -------------------------------------------                                                                                                  
    ASPIRIN                     22           36                                                                                                  
    IBUPROEN                     5            6                                                                                                  
    Placebo                                                                                                                                      
                                                                                                                                                 
    Type 'unq race' and this will appear in the output window                                                                                    
                                                                                                                                                 
    Dataset=_last_  frequecies and distinct count for race                                                                                       
                                   UNIQUE_                                                                                                       
    RACE                           USUBJID  OBSEVATIONS                                                                                          
    ---------------------------------------------------                                                                                          
    MARTIAN                             19           27                                                                                          
    OTHER                                6            8                                                                                          
    VENUSIAN                            13           15                                                                                          
                                                                                                                                                 
    Type 'unq trt race' and this will appear in the output window                                                                                
                                                                                                                                                 
    Dataset=_last_  frequecies and distinct count for trt race                                                                                   
                                                     UNIQUE_                                                                                     
    TRT               RACE                           USUBJID  OBSEVATIONS                                                                        
    ---------------------------------------------------------------------                                                                        
    ASPIRIN           MARTIAN                             15           18                                                                        
    ASPIRIN           OTHER                                4            5                                                                        
    ASPIRIN           VENUSIAN                            12           13                                                                        
    IBUPROEN          MARTIAN                              4            5                                                                        
    IBUPROEN          OTHER                                1            1                                                                        
    Placebo           MARTIAN                              4            4                                                                        
    Placebo           OTHER                                2            2                                                                        
    Placebo           VENUSIAN                             2            2                                                                        
                       _                                                                                                                         
     _   _ _ __   __ _| |__                                                                                                                      
    | | | | `_ \ / _` | `_ \                                                                                                                     
    | |_| | | | | (_| | | | |                                                                                                                    
     \__,_|_| |_|\__, |_| |_|                                                                                                                    
                    |_|                                                                                                                          
                                                                                                                                                 
                                                                                                                                                 
    Just add the h suffix to unq                                                                                                                 
                                                                                                                                                 
    Highlight the dataset name pain_study and type unqh trt and this will appear in output                                                       
                                                                                                                                                 
    Dataset=pain_study  frequecies and distinct count for trt                                                                                    
                                                                                                                                                 
                           UNIQUE_                                                                                                               
    TRT                    USUBJID  OBSEVATIONS                                                                                                  
    -------------------------------------------                                                                                                  
    ASPIRIN                     22           36                                                                                                  
    IBUPROEN                     5            6                                                                                                  
                                                                                                                                                 
    same fdor race and trt race                                                                                                                  
                                                                                                                                                 
                                                                                                                                                 
     _ __ ___   __ _  ___ _ __ ___  ___                                                                                                          
    | `_ ` _ \ / _` |/ __| `__/ _ \/ __|                                                                                                         
    | | | | | | (_| | (__| | | (_) \__ \                                                                                                         
    |_| |_| |_|\__,_|\___|_|  \___/|___/                                                                                                         
                                                                                                                                                 
                                                                                                                                                 
    %symdel arg var argx;                                                                                                                        
                                                                                                                                                 
    * DOSUBL simplifies command macros;                                                                                                          
                                                                                                                                                 
    %macro unqh(var)/cmd parmbuff                                                                                                                
        des="Frquency and distinct counts. Usubjid is hardcoded and is in almost all FDA SDTMs";                                                 
                                                                                                                                                 
       store;                                                                                                                                    
                                                                                                                                                 
       %let argx=&syspbuff;                                                                                                                      
                                                                                                                                                 
       %if %sysfunc(countw(&argx))=2 %then %do;                                                                                                  
            %let arg= %qsysfunc(translate(&argx,%str(,),%str( )));                                                                               
            %put &arg;                                                                                                                           
       %end;                                                                                                                                     
       %else %do;                                                                                                                                
           %let arg = &argx;                                                                                                                     
       %end;                                                                                                                                     
                                                                                                                                                 
       %let rc=%sysfunc(dosubl('                                                                                                                 
         title "Dataset=&sas_dataset  frequecies and distinct count for &argx";                                                                  
         FILENAME clp clipbrd ;                                                                                                                  
         DATA _NULL_;                                                                                                                            
           INFILE clp;                                                                                                                           
           INPUT;                                                                                                                                
           put _infile_;                                                                                                                         
           call symputx("sas_dataset",_infile_);                                                                                                 
         RUN;                                                                                                                                    
         proc sql;                                                                                                                               
            select                                                                                                                               
               &arg                                                                                                                              
              ,count(distinct(usubjid)) as UNIQUE_USUBJID                                                                                        
              ,count(*)                 as OBSEVATIONS                                                                                           
            from                                                                                                                                 
              &sas_dataset                                                                                                                       
            group                                                                                                                                
              by &arg                                                                                                                            
         ;quit;                                                                                                                                  
         '));                                                                                                                                    
                                                                                                                                                 
    %mend unqh;                                                                                                                                  
                                                                                                                                                 
                                                                                                                                                 
                                                                                                                                                 
    %macro unq(var)/cmd parmbuff                                                                                                                 
        des="Frquency and distinct counts. Usubjid is hardcoded and is in almost all FDA SDTMs";                                                 
                                                                                                                                                 
       %let argx=&syspbuff;                                                                                                                      
                                                                                                                                                 
       %if %sysfunc(countw(&argx))=2 %then %do;                                                                                                  
            %let arg= %qsysfunc(translate(&argx,%str(,),%str( )));                                                                               
            %put &arg;                                                                                                                           
       %end;                                                                                                                                     
       %else %do;                                                                                                                                
           %let arg = &argx;                                                                                                                     
       %end;                                                                                                                                     
                                                                                                                                                 
       %let rc=%sysfunc(dosubl('                                                                                                                 
         title "Dataset=_last_  frequecies and distinct count for &argx";                                                                        
         proc sql;                                                                                                                               
            select                                                                                                                               
               &arg                                                                                                                              
              ,count(distinct(usubjid)) as UNIQUE_USUBJID                                                                                        
              ,count(*)                 as OBSEVATIONS                                                                                           
            from                                                                                                                                 
              _last_                                                                                                                             
            group                                                                                                                                
              by &arg                                                                                                                            
         ;quit;                                                                                                                                  
         '));                                                                                                                                    
                                                                                                                                                 
    %mend unq;                                                                                                                                   
     _                                                                                                                                           
    | | ___   __ _                                                                                                                               
    | |/ _ \ / _` |                                                                                                                              
    | | (_) | (_| |                                                                                                                              
    |_|\___/ \__, |                                                                                                                              
             |___/                                                                                                                               
                                                                                                                                                 
    5116  data pain_study ;                                                                                                                      
    5117    length trt $16 race $24;                                                                                                             
    5118    set sashelp.cars(rename=(make=race drivetrain=trt)  keep= make drivetrain where=(race=:"C"));                                        
    5119    usubjid = int(31*uniform(1234));                                                                                                     
    5120    select (race);                                                                                                                       
    5121      when ('Cadillac '  ) race='OTHER';                                                                                                 
    5122      when ('Chevrolet'  ) race='MARTIAN';                                                                                               
    5123      when ('Chrysler '  ) race='VENUSIAN';                                                                                              
    5124    end; /* no need for otherwise */                                                                                                     
    5125    select (trt);                                                                                                                        
    5126      when ('All'  ) trt='IBUPROEN';                                                                                                     
    5127      when ('Front') trt='ASPIRIN';                                                                                                      
    5128      when ('Rear' ) trt='Placebo';                                                                                                      
    5129    end;                                                                                                                                 
    5130    pain_level=int(11*uniform(2345));                                                                                                    
    5131  run;                                                                                                                                   
                                                                                                                                                 
    NOTE: There were 50 observations read from the data set SASHELP.CARS.                                                                        
          WHERE race=:'C';                                                                                                                       
    NOTE: The data set WORK.PAIN_STUDY has 50 observations and 4 variables.                                                                      
    NOTE: DATA statement used (Total process time):                                                                                              
          real time           0.01 seconds                                                                                                       
          user cpu time       0.00 seconds                                                                                                       
          system cpu time     0.00 seconds                                                                                                       
          memory              730.78k                                                                                                            
          OS Memory           23552.00k                                                                                                          
          Timestamp           08/14/2021 02:44:20 PM                                                                                             
          Step Count                        271  Switch Count  0                                                                                 
                                                                                                                                                 
    5131!     quit;                                                                                                                              
                                                                                                                                                 
    MLOGIC(UNQH):  Beginning execution.                                                                                                          
    MLOGIC(UNQH):  Parameter VAR has value trt                                                                                                   
    MLOGIC(UNQH):  %LET (variable name is ARGX)                                                                                                  
    SYMBOLGEN:  Macro variable SYSPBUFF resolves to trt race                                                                                     
    SYMBOLGEN:  Macro variable ARGX resolves to trt race                                                                                         
    MLOGIC(UNQH):  %IF condition %sysfunc(countw(&argx))=2 is TRUE                                                                               
    MLOGIC(UNQH):  %LET (variable name is ARG)                                                                                                   
    SYMBOLGEN:  Macro variable ARGX resolves to trt race                                                                                         
    MLOGIC(UNQH):  %PUT &arg                                                                                                                     
    SYMBOLGEN:  Macro variable ARG resolves to trt,race                                                                                          
    SYMBOLGEN:  Some characters in the above value which were subject to macro quoting have been unquoted for printing.                          
    trt,race                                                                                                                                     
    SYMBOLGEN:  Macro variable SAS_DATASET resolves to pain_study                                                                                
    SYMBOLGEN:  Macro variable ARGX resolves to trt race                                                                                         
    NOTE: The infile CLP is:                                                                                                                     
          (no system-specific pathname available),                                                                                               
          (no system-specific file attributes available)                                                                                         
                                                                                                                                                 
    pain_study                                                                                                                                   
    NOTE: 1 record was read from the infile CLP.                                                                                                 
          The minimum record length was 10.                                                                                                      
          The maximum record length was 10.                                                                                                      
    NOTE: DATA statement used (Total process time):                                                                                              
          real time           0.01 seconds                                                                                                       
          user cpu time       0.00 seconds                                                                                                       
          system cpu time     0.01 seconds                                                                                                       
          memory              317.31k                                                                                                            
          OS Memory           23552.00k                                                                                                          
          Timestamp           08/14/2021 02:44:35 PM                                                                                             
          Step Count                        272  Switch Count  0                                                                                 
                                                                                                                                                 
                                                                                                                                                 
    SYMBOLGEN:  Macro variable ARG resolves to trt,race                                                                                          
    SYMBOLGEN:  Some characters in the above value which were subject to macro quoting have been unquoted for printing.                          
    SYMBOLGEN:  Macro variable SAS_DATASET resolves to pain_study                                                                                
    SYMBOLGEN:  Macro variable ARG resolves to trt,race                                                                                          
    SYMBOLGEN:  Some characters in the above value which were subject to macro quoting have been unquoted for printing.                          
    NOTE: PROCEDURE SQL used (Total process time):                                                                                               
          real time           0.04 seconds                                                                                                       
          user cpu time       0.01 seconds                                                                                                       
          system cpu time     0.03 seconds                                                                                                       
          memory              5388.71k                                                                                                           
          OS Memory           28676.00k                                                                                                          
          Timestamp           08/14/2021 02:44:35 PM                                                                                             
          Step Count                        273  Switch Count  0                                                                                 
                                                                                                                                                 
                                                                                                                                                 
    MLOGIC(UNQH):  %LET (variable name is RC)                                                                                                    
    NOTE: The quoted string currently being processed has become more than 262 characters long.  You might have unbalanced quotation marks.      
                    _                                                                                                                            
      ___ _ __   __| |                                                                                                                           
     / _ \ `_ \ / _` |                                                                                                                           
    |  __/ | | | (_| |                                                                                                                           
     \___|_| |_|\__,_|                                                                                                                           
                                                                                                                                                 
                                                                                                                                                 
                                                                                                                                                 

