# utl-proper-handling-of-nulls-when-creating-json-output-to-load-into-databases
Proper handling of nulls when creating json output to load into databases   
    Proper handling of nulls when creating json output to load into databases                                                      
                                                                                                                                   
      Convert SAS table to JSON                                                                                                    
                                                                                                                                   
      Cannot see a workaround with SAS 'proc json'. Aolution in R.                                                                 
                                                                                                                                   
    github                                                                                                                         
    https://tinyurl.com/y4ubo4jl                                                                                                   
    https://github.com/rogerjdeangelis/utl-proper-handling-of-nulls-when-creating-json-output-to-load-into-databases               
                                                                                                                                   
    SAS Forum                                                                                                                      
    https://tinyurl.com/y3x4nhld                                                                                                   
    https://communities.sas.com/t5/SAS-Programming/Export-SAS-dataset-to-JSON-with-missing-string-variable-as-null/m-p/556059      
                                                                                                                                   
    macros                                                                                                                         
    https://tinyurl.com/y9nfugth                                                                                                   
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                     
                                                                                                                                   
    *                _     _                                                                                                       
     _ __  _ __ ___ | |__ | | ___ _ __ ___                                                                                         
    | '_ \| '__/ _ \| '_ \| |/ _ \ '_ ` _ \                                                                                        
    | |_) | | | (_) | |_) | |  __/ | | | | |                                                                                       
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|                                                                                       
    |_|                                                                                                                            
    ;                                                                                                                              
                                                                                                                                   
    libname sd1 "d:/sd1";                                                                                                          
                                                                                                                                   
    data sd1.have;                                                                                                                 
       length status $2.;                                                                                                          
       INFILE DATALINES DLM='|';                                                                                                   
       Length CODE 3 Name $ 50;                                                                                                    
       Input CODE Name;                                                                                                            
       if name = "Joe" then                                                                                                        
          do;                                                                                                                      
            status="";   ** Missing character;                                                                                     
            code=.;      ** Missing numeric code;                                                                                  
          end;                                                                                                                     
          else status="Y";                                                                                                         
    cards4;                                                                                                                        
    11|Bob                                                                                                                         
    12|Joe                                                                                                                         
    13|Larry                                                                                                                       
    ;;;;                                                                                                                           
    run;quit;                                                                                                                      
                                                                                                                                   
    proc json out="d:\json\want_sas.json" pretty NOSASTAGS;                                                                        
       export sd1.have;                                                                                                            
    run;                                                                                                                           
                                                                                                                                   
                                                                                                                                   
    [                                                                                                                              
      {                                                                                                                            
        "status": "Y",                                                                                                             
        "CODE": 11,                                                                                                                
        "Name": "Bob"                                                                                                              
      },                                                                                                                           
      {                                                                                                                            
        "status": "",   *** THIS SHOUD BE NULL;                                                                                    
        "CODE": null,                                                                                                              
        "Name": "Joe"                                                                                                              
      },                                                                                                                           
      {                                                                                                                            
        "status": "Y",                                                                                                             
        "CODE": 13,                                                                                                                
        "Name": "Larry"                                                                                                            
      }                                                                                                                            
    ]                                                                                                                              
                                                                                                                                   
    *_                   _                                                                                                         
    (_)_ __  _ __  _   _| |_                                                                                                       
    | | '_ \| '_ \| | | | __|                                                                                                      
    | | | | | |_) | |_| | |_                                                                                                       
    |_|_| |_| .__/ \__,_|\__|                                                                                                      
            |_|                                                                                                                    
    ;                                                                                                                              
                                                                                                                                   
    libname sd1 "d:/sd1";                                                                                                          
                                                                                                                                   
    data sd1.have;                                                                                                                 
       length status $2.;                                                                                                          
       INFILE DATALINES DLM='|';                                                                                                   
       Length CODE 3 Name $ 50;                                                                                                    
       Input CODE Name;                                                                                                            
       if name = "Joe" then                                                                                                        
          do;                                                                                                                      
            status="";                                                                                                             
            code=.;                                                                                                                
          end;                                                                                                                     
          else status="Y";                                                                                                         
    cards4;                                                                                                                        
    11|Bob                                                                                                                         
    12|Joe                                                                                                                         
    13|Larry                                                                                                                       
    ;;;;                                                                                                                           
    run;quit;                                                                                                                      
                                                                                                                                   
    Up to 40 obs SD1.HAVE total obs=3                                                                                              
                                                                                                                                   
    Obs    STATUS    CODE    NAME                                                                                                  
                                                                                                                                   
     1       Y        11     Bob                                                                                                   
     2                 .     Joe                                                                                                   
     3       Y        13     Larry                                                                                                 
                                                                                                                                   
    *            _               _                                                                                                 
      ___  _   _| |_ _ __  _   _| |_                                                                                               
     / _ \| | | | __| '_ \| | | | __|                                                                                              
    | (_) | |_| | |_| |_) | |_| | |_                                                                                               
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                              
                    |_|                                                                                                            
    ;                                                                                                                              
                                                                                                                                   
                                                                                                                                   
    d:/json/want_r.txt                                                                                                             
                                                                                                                                   
                                                                                                                                   
    [                                                                                                                              
      {                                                                                                                            
        "STATUS": "Y",                                                                                                             
        "CODE": 11,                                                                                                                
        "NAME": "Bob"                                                                                                              
      },                                                                                                                           
      {                                                                                                                            
        "STATUS": null,   * both are null - will load into most databases?;                                                        
        "CODE": null,     * numm numeric - SAS only handles this on?;                                                              
        "NAME": "Joe"                                                                                                              
      },                                                                                                                           
      {                                                                                                                            
        "STATUS": "Y",                                                                                                             
        "CODE": 13,                                                                                                                
        "NAME": "Larry"                                                                                                            
      }                                                                                                                            
    ]>                                                                                                                             
                                                                                                                                   
    *                                                                                                                              
     _ __  _ __ ___   ___ ___  ___ ___                                                                                             
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                            
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                            
    | .__/|_|  \___/ \___\___||___/___/                                                                                            
    |_|                                                                                                                            
    ;                                                                                                                              
                                                                                                                                   
    %utlfkil(d:/json/want_r.txt); *just to be safe;                                                                                
                                                                                                                                   
    %utl_submit_r64('                                                                                                              
    library(haven);                                                                                                                
    library(jsonlite);                                                                                                             
    have<-read_sas("d:/sd1/have.sas7bdat");                                                                                        
    have$STATUS[have$NAME=="Joe"] <- NA;                                                                                           
    want<-toJSON(have, na = "null",  pretty = TRUE, dataframe = "rows");                                                           
    cat(want);                                                                                                                     
    cat(want, sep = "\n", file = "d:/json/want_r.txt");                                                                            
    ');                                                                                                                            
                                                                                                                                   
