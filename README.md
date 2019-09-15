# utl-select-the-unique-largest-value-that-has-a--frequency-of-one
    Select the unique largest value that has a  frequency of one                                                     
                                                                                                                     
     Problem:                                                                                                        
                                                                                                                     
            1                                                                                                        
            2                                                                                                        
            2                                                                                                        
                                                                                                                     
            5  * 5 is largest value with frequency=1                                                                 
                 9 is larger but has frequenc=2                                                                      
            9                                                                                                        
            9                                                                                                        
                                                                                                                     
       Seven Solutions                                                                                               
                                                                                                                     
              a. sql arrays                                                                                          
                                                                                                                     
              b. hash (I am a novice hash programmer, but Paul's Book                                                
                 'Data Management Solutions Using SAS Hash Table Operations'                                         
                 was really helpful (chapter 4)                                                                      
                                                                                                                     
              c. dosubl                                                                                              
                                                                                                                     
              d. R one liner.                                                                                        
                                                                                                                     
              e. Pure HASH (no dusubl )                                                                              
                 Bartosz Jablonski                                                                                   
                 yabwon@gmail.com                                                                                    
                                                                                                                     
              f. Olde Key Index                                                                                      
                 Paul Dorfman                                                                                        
                 sashole@bellsouth.net                                                                               
                                                                                                                     
              g. Very clever uses the update property of  collapsing sparse arrays                                   
                 Richard DeVenezia                                                                                   
                 rdevenezia@gmail.com                                                                                
                 (Hope your don't mind, I packaged the solution in one datastep)                                     
                 (see explanation below)                                                                             
                                                                                                                     
    github                                                                                                           
    https://tinyurl.com/y6yqrqhu                                                                                     
    https://github.com/rogerjdeangelis/utl-select-the-unique-largest-value-that-has-a-frequency-of-one               
                                                                                                                     
    https://stackoverflow.com/questions/57790807/how-to-find-a-unique-maximum                                        
                                                                                                                     
    Jason Aizkalns profile                                                                                           
    https://stackoverflow.com/users/2572423/jasonaizkalns                                                            
                                                                                                                     
    *_                   _                                                                                           
    (_)_ __  _ __  _   _| |_                                                                                         
    | | '_ \| '_ \| | | | __|                                                                                        
    | | | | | |_) | |_| | |_                                                                                         
    |_|_| |_| .__/ \__,_|\__|                                                                                        
            |_|                                                                                                      
    ;                                                                                                                
                                                                                                                     
    options validvarname=upcase;                                                                                     
    libname sd1 "d:/sd1";                                                                                            
    data sd1.have;                                                                                                   
                                                                                                                     
      array nums[10] n1-n10;                                                                                         
                                                                                                                     
      do rec=1 to 10;                                                                                                
         do idx=1 to 10;                                                                                             
            nums[idx]=int(8*uniform(12345));                                                                         
         end;                                                                                                        
         output;                                                                                                     
      end;                                                                                                           
                                                                                                                     
      drop rec idx;                                                                                                  
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
                                                                                                                     
                                                                                                                     
     SD1.HAVE total obs=10                                                                                           
                                                                                                                     
     N1    N2    N3    N4    N5    N6    N7    N8    N9    N10                                                       
                                                                                                                     
      2     5*    6     2     1     5     0     5     5     6                                                        
      3     1     1     3     3     6     3     1     1     1                                                        
      2     4     2     6     4     2     1     0     7     1                                                        
      4     0     6     7*    2     1     3     1     5     0                                                        
      6     6     4*    1     2     5     5     6     1     7                                                        
      0     6     3     4     2     3     2     3     3*    3*                                                       
      6     6     3     4     0     6     7*    5     7     3                                                        
      5*    3     5     4     4     5     5     5     6     3                                                        
      4     4     6     0     5*    4     2     7*    6     1                                                        
      1     4     5     2     0     7*    6     6     0     3                                                        
                                                                                                                     
     *           _               _                                                                                   
      ___  _   _| |_ _ __  _   _| |_                                                                                 
     / _ \| | | | __| '_ \| | | | __|                                                                                
    | (_) | |_| | |_| |_) | |_| | |_                                                                                 
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                
                    |_|                                                                                              
    ;                                                                                                                
                                                                                                                     
    WORK.WANT total obs=10                                                                                           
                                                                                                                     
      LARGET_                                                                                                        
      UNIQUE    Largest_Unique                                                                                       
                                                                                                                     
        N1            5                                                                                              
        N2            5                                                                                              
        N3            4                                                                                              
        N4            7                                                                                              
        N5            5                                                                                              
        N6            7                                                                                              
        N7            7                                                                                              
        N8            7                                                                                              
        N9            3                                                                                              
        N10           7                                                                                              
                                                                                                                     
    *          _       _   _                                                                                         
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                                         
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                                        
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                                        
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                                        
                          _                                                                                          
      __ _      ___  __ _| |                                                                                         
     / _` |    / __|/ _` | |                                                                                         
    | (_| |_   \__ \ (_| | |                                                                                         
     \__,_(_)  |___/\__, |_|                                                                                         
                       |_|                                                                                           
    ;                                                                                                                
                                                                                                                     
    * could br fast with 10 cores;                                                                                   
    %array(nums,values=%utl_varlist(sd1.have));                                                                      
                                                                                                                     
    proc sql;                                                                                                        
       create                                                                                                        
           table want as                                                                                             
       %do_over(nums,phrase=%str(                                                                                    
          select "?" as larget_unique,                                                                               
              max(?) as big                                                                                          
          from                                                                                                       
            (select ? from sd1.have group by ? having count(n1)=1)),                                                 
          between=union all                                                                                          
          )                                                                                                          
    ;quit;                                                                                                           
                                                                                                                     
    *_         _               _                                                                                     
    | |__     | |__   __ _ ___| |__                                                                                  
    | '_ \    | '_ \ / _` / __| '_ \                                                                                 
    | |_) |   | | | | (_| \__ \ | | |                                                                                
    |_.__(_)  |_| |_|\__,_|___/_| |_|                                                                                
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
    sasfile sd1.have load;  * important because read 10 times;                                                       
                                                                                                                     
    %symdel bigung / nowarn;                                                                                         
                                                                                                                     
    data want;                                                                                                       
                                                                                                                     
      do ns=1 to 10;                                                                                                 
                                                                                                                     
         var=cats('n',ns);                                                                                           
                                                                                                                     
         call symputx ('n',ns);                                                                                      
                                                                                                                     
         rc=dosubl('                                                                                                 
           data _null_;                                                                                              
                                                                                                                     
              if _n_ = 1 then do;                                                                                    
                 declare hash h( ordered:"y");                                                                       
                 h.definekey("n&n");                                                                                 
                 h.definedata("n&n", "count");                                                                       
                 declare hiter ih("h");                                                                              
                 h.definedone();                                                                                     
              end;                                                                                                   
                                                                                                                     
              do until(last);                                                                                        
                 set sd1.have end = last;                                                                            
                 if h.find() ^= 0 then count = 0;                                                                    
                 count + 1;                                                                                          
                 h.replace();                                                                                        
              end;                                                                                                   
                                                                                                                     
              do rc=ih.last() by 0 while(rc=0);                                                                      
                 if count=1 then do;                                                                                 
                     call symputx("bigUnq",n&n);                                                                     
                     stop;                                                                                           
                 end;                                                                                                
                 rc=ih.prev();                                                                                       
              end;                                                                                                   
                                                                                                                     
           run;quit;                                                                                                 
         ');                                                                                                         
                                                                                                                     
         largest_unique=symgetn("bigUnq");                                                                           
         drop ns rc;                                                                                                 
         output;                                                                                                     
       end;                                                                                                          
                                                                                                                     
       stop;                                                                                                         
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
    sasfile sd1 close;                                                                                               
                                                                                                                     
                                                                                                                     
    *             _                 _     _                                                                          
      ___      __| | ___  ___ _   _| |__ | |                                                                         
     / __|    / _` |/ _ \/ __| | | | '_ \| |                                                                         
    | (__ _  | (_| | (_) \__ \ |_| | |_) | |                                                                         
     \___(_)  \__,_|\___/|___/\__,_|_.__/|_|                                                                         
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
    data want;                                                                                                       
                                                                                                                     
      if _n_0 then do; %let rc=%sysfunc(dosubl('                                                                     
         ods exclude all;                                                                                            
         ods output Frequencies=freq(drop=cell cum where=(count=1));                                                 
         proc univariate data=sd1.have freq;                                                                         
            var n:;                                                                                                  
         run;                                                                                                        
         ods select all;                                                                                             
         '));                                                                                                        
      end;                                                                                                           
                                                                                                                     
      set freq;                                                                                                      
        by varname notsorted;                                                                                        
        keep varname value;                                                                                          
        if last.varname then output;                                                                                 
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
    *    _     ____                                                                                                  
      __| |   |  _ \                                                                                                 
     / _` |   | |_) |                                                                                                
    | (_| |_  |  _ <                                                                                                 
     \__,_(_) |_| \_\                                                                                                
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
                                                                                                                     
    %utl_submit_r64('                                                                                                
    library(haven);                                                                                                  
    library(SASxport);                                                                                               
    library(haven);                                                                                                  
    have<-read_sas("d:/sd1/have.sas7bdat");                                                                          
    want<-as.data.frame(apply(have,2,function(x) {max(names(which(table(x) == 1)))}));                               
    want[] <- lapply(want, function(x) if(is.factor(x)) as.character(x) else x);                                     
    str(want);                                                                                                       
    write.xport(want,file="d:/xpt/want.xpt");                                                                        
    ');                                                                                                              
                                                                                                                     
    libname xpt xport "d:/xpt/want.xpt";                                                                             
    data want;                                                                                                       
      retain var;                                                                                                    
      set xpt.want;                                                                                                  
      var=cats('n',_n_);                                                                                             
    run;quit;                                                                                                        
    libname xpt clear;                                                                                               
    *         ____             _                                                                                     
      ___    | __ )  __ _ _ __| |_                                                                                   
     / _ \   |  _ \ / _` | '__| __|                                                                                  
    |  __/_  | |_) | (_| | |  | |_                                                                                   
     \___(_) |____/ \__,_|_|   \__|                                                                                  
                                                                                                                     
    ;                                                                                                                
    1) Paul and Don's hashbook is a masterpiece! I know what I'm saying I spent a lot of time with the book.         
                                                                                                                     
    2) Let me share Hash-of-Hashes approach (with both vertical and horizontal output).                              
                                                                                                                     
    All the best                                                                                                     
    Bart                                                                                                             
                                                                                                                     
    data horizonatal(keep = n:) vertical(drop = n: cnt);                                                             
      length VN $ 32 val cnt 8;                                                                                      
      declare hash H;                                                                                                
                                                                                                                     
      declare hash HoH();                                                                                            
      HoH.defineKey('VN');                                                                                           
      HoH.defineData('VN','H');                                                                                      
      HoH.defineDone();                                                                                              
                                                                                                                     
      do until(EOF); /* loop over set */                                                                             
        set sd1.have end = EOF;                                                                                      
        array nums n:;                                                                                               
        do over nums ; /* loop over array */                                                                         
                                                                                                                     
          VN = vname(nums);                                                                                          
          if HoH.find() then /* test if variable name was marked in HoH*/                                            
            do; /* if not add it and create new instance of H - for a given variable nXX*/                           
              H = _new_ hash(ordered:"D");                                                                           
              H.defineKey('val');                                                                                    
              H.defineData('val','cnt');                                                                             
              H.defineDone();                                                                                        
                                                                                                                     
              HoH.add();                                                                                             
            end;                                                                                                     
          val = nums; /* get the nXX value and count */                                                              
          if H.find() then cnt = 1;                                                                                  
                      else cnt + 1;                                                                                  
          _N_ = H.replace();                                                                                         
                                                                                                                     
        end;                                                                                                         
      end;                                                                                                           
                                                                                                                     
      do over nums ; /* loop through array*/                                                                         
        nums = .; /* if no single max case (like 9 9 8 8 7 7) */                                                     
        _N_ = HoH.find(key:vname(nums));                                                                             
                                                                                                                     
        declare hiter I("H");                                                                                        
        do while(I.next() = 0);                                                                                      
          if cnt = 1 then                                                                                            
            do;                                                                                                      
              nums = val;                                                                                            
              output vertical; /*for vertical output; */                                                             
              leave;                                                                                                 
            end;                                                                                                     
        end;                                                                                                         
        _N_ = I.delete();                                                                                            
      end;                                                                                                           
      output horizonatal; /*for horizontal output; */                                                                
                                                                                                                     
    stop;                                                                                                            
                                                                                                                     
    run;quit;                                                                                                        
    * __    ____             _                                                                                       
     / _|  |  _ \ __ _ _   _| |                                                                                      
    | |_   | |_) / _` | | | | |                                                                                      
    |  _|  |  __/ (_| | |_| | |                                                                                      
    |_|(_) |_|   \__,_|\__,_|_|                                                                                      
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
    Bart,                                                                                                            
                                                                                                                     
    First, thanks for the generous plug. Second, this program of                                                     
    yours is in itself a masterpiece showing how HoH should be used when                                             
    HoH is called for by the logic of the program.                                                                   
                                                                                                                     
    The hash object rules where you want dynamic run-time processing without                                         
    relying on pre-processing and making assumptions about data. Still, I                                            
    can't help but note that as long as the maximum of the count values stays                                        
    within a reasonable integer limit, a modicum of goode olde key-indexing                                          
    (which, after all, is a degenerative case of hashing)                                                            
    not only doesn't hurt but also makes for                                                                         
    a pretty neat implementation. Below, I combined Maxcount and Nrecords                                            
    to find a case where some N's have no non-duplicate values.                                                      
                                                                                                                     
    %let maxcount =   75000 ;                                                                                        
    %let nrecords = 1000000 ;                                                                                        
                                                                                                                     
    data have ;                                                                                                      
      call streaminit (7) ;                                                                                          
      array nums N1-N10 ;                                                                                            
      do _n_ = 1 to &nrecords ;                                                                                      
        do over nums ;                                                                                               
          nums = rand ("integer", &maxcount) ;                                                                       
        end ;                                                                                                        
        output ;                                                                                                     
      end ;                                                                                                          
    run ;                                                                                                            
                                                                                                                     
    data hor (keep = N:) ver (keep = V:) ;                                                                           
      set have end = z ;                                                                                             
      array nn N: ;                                                                                                  
      array h [10, -1:&maxcount] _temporary_ ;                                                                       
      do over nn ;                                                                                                   
        if h [_i_,nn] = . then do ;                                                                                  
          h [_i_,nn] = nn ;                                                                                          
          h [_i_,-1] = h [_i_,-1] max nn ;                                                                           
        end ;                                                                                                        
        else h [_i_, nn] = ._ ;                                                                                      
      end ;                                                                                                          
      if z ;                                                                                                         
      do over nn ;                                                                                                   
        do _n_ = h [_i_,-1] by -1 to 0 until (h [_i_,_n_]) ;                                                         
        end ;                                                                                                        
        VNAME = put (vname (nn), $32.) ;                                                                             
        VAL = ifn (_n_ = -1, ., _n_) ;                                                                               
        output ver ;                                                                                                 
        nn = val ;                                                                                                   
      end ;                                                                                                          
      output hor ;                                                                                                   
    run ;                                                                                                            
                                                                                                                     
    Of course, this step and yours generate the same output. Yet to appreciate the agility                           
    of key-indexing, run your step against this input for a comparison. Admittedly,                                  
    key-indexing is limited memory-wise by the Maxcount and logically-wise -                                         
    by assuming that the counts are numeric and integer. The hash object program has no such limitations.            
                                                                                                                     
    Best regards                                                                                                     
                                                                                                                     
    *          ____  _      _                   _                                                                    
      __ _    |  _ \(_) ___| |__   __ _ _ __ __| |                                                                   
     / _` |   | |_) | |/ __| '_ \ / _` | '__/ _` |                                                                   
    | (_| |_  |  _ <| | (__| | | | (_| | | | (_| |                                                                   
     \__, (_) |_| \_\_|\___|_| |_|\__,_|_|  \__,_|                                                                   
     |___/                                                                                                           
    ;                                                                                                                
                                                                                                                     
    * Make input table;                                                                                              
                                                                                                                     
    data sd1.have;                                                                                                   
                                                                                                                     
      array nums[10] n1-n10;                                                                                         
                                                                                                                     
      do rec=1 to 10;                                                                                                
         do idx=1 to 10;                                                                                             
            nums[idx]=int(8*uniform(12345));                                                                         
         end;                                                                                                        
         output;                                                                                                     
      end;                                                                                                           
                                                                                                                     
      drop rec idx;                                                                                                  
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
    data want;                                                                                                       
                                                                                                                     
      if _n_=0 then do; %let rc=%sysfunc(dosubl('                                                                    
                                                                                                                     
         ods exclude all;                                                                                            
         ods output onewayfreqs=havFrq (where=(frequency=1));                                                        
                                                                                                                     
         proc freq data=have;                                                                                        
           table n: ;                                                                                                
         run;quit;                                                                                                   
                                                                                                                     
         ods select all;                                                                                             
                                                                                                                     
         data want;                                                                                                  
           array n n1-n10;                                                                                           
           frequency=1;                                                                                              
         run;quit;                                                                                                   
                                                                                                                     
         '));                                                                                                        
      end;                                                                                                           
                                                                                                                     
      update want havFrq(keep=frequency n:);                                                                         
      by frequency;                                                                                                  
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
                                                                                                                     
    * Explanation                                                                                                    
                                                                                                                     
    The key to Richard solution is a property of update.                                                             
    These are the unique counts that are candidates for                                                              
    the unique maximum                                                                                               
                                                                                                                     
     N1   N2   N3  Frequency                                                                                         
                                                                                                                     
      0    .    .     1                                                                                              
      1    .    .     1                                                                                              
      3    .    .     1                                                                                              
      5    .    .     1                                                                                              
      .    1    .     1                                                                                              
      .    2    .     1                                                                                              
      .    4    .     1                                                                                              
                                                                                                                     
    ...                                                                                                              
                                                                                                                     
    Update does this                                                                                                 
                                                                                                                     
     N1   N2   N3  Frequency                                                                                         
                                                                                                                     
      0    1    1     1                                                                                              
      1    2    3     1                                                                                              
      3    4    4     1                                                                                              
      5    .    .     1  * when want has ine ob this outputs the last ob                                             
                         for each n,                                                                                 
                                                                                                                     
    When want has just on ob it output                                                                               
    the ns;                                                                                                          
                                                                                                                     
                                                                                                                     
