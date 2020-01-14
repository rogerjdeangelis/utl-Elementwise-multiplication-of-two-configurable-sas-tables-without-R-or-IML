# utl-Elementwise-multiplication-of-two-configurable-sas-tables-without-R-or-IML
Elementwise multiplication of two configurable sas tables without R or IML
    Elementwise multiplication of two configurable sas tables without R or IML                                                    
                                                                                                                                  
    Very elegant solution by                                                                                                      
    Mark Keintz <mkeintz@WHARTON.UPENN.EDU>                                                                                       
    https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;8d3a351e.2001b                                                                   
                                                                                                                                  
    github                                                                                                                        
    https://tinyurl.com/u7rmynw                                                                                                   
    https://github.com/rogerjdeangelis/utl-Elementwise-multiplication-of-two-configurable-sas-tables-without-R-or-IML             
                                                                                                                                  
    related repo                                                                                                                  
    https://tinyurl.com/qrmpxzo                                                                                                   
    https://github.com/rogerjdeangelis/utl-renamimg-coordinated-lists-of-of-column-names                                          
                                                                                                                                  
    *_                   _                                                                                                        
    (_)_ __  _ __  _   _| |_                                                                                                      
    | | '_ \| '_ \| | | | __|                                                                                                     
    | | | | | |_) | |_| | |_                                                                                                      
    |_|_| |_| .__/ \__,_|\__|                                                                                                     
            |_|                                                                                                                   
    ;                                                                                                                             
                                                                                                                                  
    data have1 have2;                                                                                                             
                                                                                                                                  
     retain id;                                                                                                                   
                                                                                                                                  
     call streaminit(4321);                                                                                                       
                                                                                                                                  
     array alphas[3] A B C;                                                                                                       
                                                                                                                                  
     do id = 1 to 5;                                                                                                              
                                                                                                                                  
        do ltr=1 to dim(alphas);                                                                                                  
           alphas[ltr] = RAND("Bernoulli", .50) +1 ;  /* 1s with .25 probability */                                               
        end;                                                                                                                      
                                                                                                                                  
        output have1;                                                                                                             
                                                                                                                                  
        do ltr=1 to dim(alphas);                                                                                                  
           alphas[ltr] = int(4*RAND("Uniform")) + 1;                                                                              
        end;                                                                                                                      
                                                                                                                                  
        output have2;                                                                                                             
     end;                                                                                                                         
     drop ltr ;                                                                                                                   
     stop;                                                                                                                        
                                                                                                                                  
    run;quit;                                                                                                                     
                                                                                                                                  
                                                                                                                                  
    WORK.HAVE1 total obs=5                                                                                                        
                                                                                                                                  
     ID    A    B    C                                                                                                            
                                                                                                                                  
      1    1    2    1                                                                                                            
      2    2    2    2                                                                                                            
      3    1    2    2                                                                                                            
      4    2    2    1                                                                                                            
      5    1    1    1                                                                                                            
                                                                                                                                  
    WORK.HAVE2 total obs=5                                                                                                        
                                                                                                                                  
     ID    A    B    C                                                                                                            
                                                                                                                                  
      1    1    1    3                                                                                                            
      2    2    4    1                                                                                                            
      3    3    2    4                                                                                                            
      4    3    4    4                                                                                                            
      5    2    2    4                                                                                                            
                                                                                                                                  
                                                                                                                                  
    *            _               _                                                                                                
      ___  _   _| |_ _ __  _   _| |_                                                                                              
     / _ \| | | | __| '_ \| | | | __|                                                                                             
    | (_) | |_| | |_| |_) | |_| | |_                                                                                              
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                             
                    |_|                                                                                                           
    ;                                                                                                                             
                          | RULES                                                                                                 
    WORK.WANT total obs=5 |                                                                                                       
                          | HAVE1  HAVE2  PRODUCT                                                                                 
                                                                                                                                  
      ID    A    B    C   |  C       C       C                                                                                    
                                                                                                                                  
       1    1    2    3   |  1       3       3                                                                                    
       2    4    8    2   |  2       1       2                                                                                    
       3    3    4    8   |  2       4       8                                                                                    
       4    6    8    4   |  1       4       4                                                                                    
       5    2    2    4   |  1       4       4                                                                                    
                                                                                                                                  
    *          _       _   _                                                                                                      
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                           
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                          
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                         
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                         
                                                                                                                                  
    ;                                                                                                                             
                                                                                                                                  
    data want;                                                                                                                    
      set have1 have2;                                                                                                            
      by id;                                                                                                                      
      array vars A--C;                                                                                                            
      do over vars;                                                                                                               
        vars=vars*lag(vars);                                                                                                      
      end;                                                                                                                        
      if last.id then output; /* you can do things here like sum the product */                                                   
    run;                                                                                                                          
                                                                                                                                  
                                                                                                                                  
