# utl-converting-a-two-deep-nested-json-file-to-retangular-sas-tables
Converting a two deep nested json file to retangular sas tables
    Converting a two deep nested json file to retangular sas tables;                                                      
                                                                                                                          
    Here is the prettyfied version of your JSON file                                                                      
                                                                                                                          
                                                                                                                          
    {                                                                                                                     
     "yaxis": 128,                                                                                                        
     "xaxis": 120,                                                                                                        
     "col"  : [11, 18, 19, 10, 18, 11],                                                                                   
     "val"  : [14.3, 9.7, 6.8, 6.2, 2.1, 5.2],                                                                            
     "row"  : [43, 52, 52, 52, 53, 54]                                                                                    
    }                                                                                                                     
                                                                                                                          
                                                                                                                          
    github                                                                                                                
    https://tinyurl.com/y3n5cvzt                                                                                          
    https://github.com/rogerjdeangelis/utl-converting-a-two-deep-nested-json-file-to-retangular-sas-tables                
                                                                                                                          
    SAS Forum                                                                                                             
    https://tinyurl.com/y3n5cvzt                                                                                          
    https://communities.sas.com/t5/SAS-Programming/Infile-for-one-long-line-of-data/m-p/580647                            
                                                                                                                          
    oter jso repos                                                                                                        
    https://tinyurl.com/y4x4dy8r                                                                                          
    https://github.com/rogerjdeangelis?utf8=%E2%9C%93&tab=repositories&q=json+in%3Aname&type=&language=                   
                                                                                                                          
    *_                   _                                                                                                
    (_)_ __  _ __  _   _| |_                                                                                              
    | | '_ \| '_ \| | | | __|                                                                                             
    | | | | | |_) | |_| | |_                                                                                              
    |_|_| |_| .__/ \__,_|\__|                                                                                             
            |_|                                                                                                           
    ;                                                                                                                     
                                                                                                                          
    I shortened some of your values so the line was shorter.                                                              
    No loss in generality.                                                                                                
                                                                                                                          
    filename ft15f001 "d:/json/two_deep.json";                                                                            
    parmcards4;                                                                                                           
    {"yaxis":128,"xaxis":120,"col":[11,18,19,10,18,11],"val":[14.3,9.7,6.8,6.2,2.1,5.2],"row":[43,52,52,52,53,54]}        
    ;;;;                                                                                                                  
    run;quit;                                                                                                             
                                                                                                                          
                                                                                                                          
    d:/json/two_deep.json                                                                                                 
                                                                                                                          
    {"yaxis":128,"xaxis":120,"col":[11,18,19,10,18,11],"val":[14.3,9.7,6.8,6.2,2.1,5.2],"row":[43,52,52,52,53,54]}        
                                                                                                                          
    *            _               _                                                                                        
      ___  _   _| |_ _ __  _   _| |_ ___                                                                                  
     / _ \| | | | __| '_ \| | | | __/ __|                                                                                 
    | (_) | |_| | |_| |_) | |_| | |_\__ \                                                                                 
     \___/ \__,_|\__| .__/ \__,_|\__|___/                                                                                 
                    |_|                                                                                                   
    ;                                                                                                                     
                                                                                                                          
     WORK.WANT total obs=5                                                                                                
                                                                                                                          
       V1       V2     V3     V4     V5     V6     V7                                                                     
                                                                                                                          
      yaxis    128     128    128    128    128    128                                                                    
      xaxis    120     120    120    120    120    120                                                                    
      col      11      18     19     10     18     11                                                                     
      val      14.3    9.7    6.8    6.2    2.1    5.2                                                                    
      row      43      52     52     52     53     54                                                                     
                                                                                                                          
                                                                                                                          
     WORK.WANTXPO total obs=6                                                                                             
                                                                                                                          
      YAXIS    XAXIS    COL    VAL     ROW                                                                                
                                                                                                                          
       128      120     11     14.3    43                                                                                 
       128      120     18     9.7     52                                                                                 
       128      120     19     6.8     52                                                                                 
       128      120     10     6.2     52                                                                                 
       128      120     18     2.1     53                                                                                 
       128      120     11     5.2     54                                                                                 
                                                                                                                          
    *          _       _   _                                                                                              
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                   
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                  
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                 
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                 
                                                                                                                          
    ;                                                                                                                     
                                                                                                                          
    %utl_submit_r64('                                                                                                     
      library("rjson");                                                                                                   
      library(SASxport);                                                                                                  
      jsn <- as.data.frame(fromJSON(paste(readLines("d:/json/two_deep.json")                                              
      ,collapse="")));                                                                                                    
      want<-as.data.frame(cbind(names(jsn),t(jsn)));                                                                      
      want[] <- lapply(want, as.character);                                                                               
      str(want);                                                                                                          
      write.xport(want,file="d:/xpt/jsnxpo.xpt");                                                                         
    ');                                                                                                                   
                                                                                                                          
    libname xpt xport "d:/xpt/jsnxpo.xpt";                                                                                
                                                                                                                          
    proc contents data=xpt._all_;                                                                                         
    run;quit;                                                                                                             
                                                                                                                          
    data want;                                                                                                            
      set xpt.want;                                                                                                       
    run;quit;                                                                                                             
                                                                                                                          
    proc print data=want;                                                                                                 
    run;quit;                                                                                                             
                                                                                                                          
    proc transpose data=want out=wantxpo(drop=_:);                                                                        
    id v1;                                                                                                                
    var v2-v7;                                                                                                            
    run;quit;                                                                                                             
                                                                                                                          
