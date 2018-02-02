# utl_dynamic_subroutines_dosubl_with_error_checking
Dynamic subroutines_dosubl_with error checking. Write a dynamic loop to join arbitrary sets of tables and provide a log and user defined condition to stop consecutive joins if certain errors occur.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Dynamic subroutines_dosubl_with error checking

      Problem:
        Write a dynamic loop to join arbirary sets of tables and provide
        a log and user defined condition to stop consecutive joins if certain
        errors occur.

      see
      https://goo.gl/X31bPs
      https://communities.sas.com/t5/Base-SAS-Programming/How-do-I-write-a-dynamic-do-loop-macro/m-p/433598


    INPUT
    =====

      INPUT OPTIONS ( I use number one)

        1. Use an array to hold the macro arguments (I prefer this)
        2. Wrap the outer dataset in a macro
        3. Use a table to hold arguments ( much less code - good option)

       array dsns[2,4] $32 x1-x20
           (
             "sashelp.class","sashelp.classfit","class1","name"
             "sashelp.class","sashelp.classfit","class2","name"
             "sashelp.qtr111","sashelp.qtr1001","qtr1","t"
             "sashelp.prdsal3","sashelp.mdv","sal1","t"
             "sashelp.qtr111","sashelp.qtr1001","qtr2","t"


        ALGORITHM

         Inner join dsn1 with dsn2 using join key and output out dsn

         BUSINESS RULES ARE TO PRODUCE THE OUTPUT

         WORK.LOG total obs=5

         STATUS          MESSAGE             IN_DSN1            IN_DSN2         OUT_DSN    JOIN_KEY

         Success    19 obs after join    sashelp.class      sashelp.classfit    class1     name
         Success    19 obs after join    sashelp.class      sashelp.classfit    class2     name
         Success    57 obs after join    sashelp.qtr111     sashelp.qtr1001     qtr1       t
         Failed     0 obs after join     sashelp.prdsal3    sashelp.mdv         sal1       country
         Success    57 obs after join    sashelp.qtr111     sashelp.qtr1001     qtr2       t


    PROCESS  (all the code)
    =======

     data log(drop=x: cal rc);

       retain status message;

       * arguments for two "macro" calls;
       array dsns[5,4] $32 x1-x20
           (
             "sashelp.class","sashelp.classfit","class1","name"
            ,"sashelp.class","sashelp.classfit","class2","name"
            ,"sashelp.qtr111","sashelp.qtr1001","qtr1","t"
            ,"sashelp.prdsal3","sashelp.mdv","sal1","country"
            ,"sashelp.qtr111","sashelp.qtr1001","qtr2","t"
           );

       do cal=1 to 5;

         * need to make sure these are in the log;

         in_dsn1  = dsns[cal,1];
         in_dsn2  = dsns[cal,2];
         out_dsn  = dsns[cal,3];
         join_key = dsns[cal,4];

         * pass to dosubl;
         * note we could just pass the address of the dsns array;

         call symputx("in_dsn1 ",dsns[cal,1]);
         call symputx("in_dsn2 ",dsns[cal,2]);
         call symputx("out_dsn ",dsns[cal,3]);
         call symputx("join_key",dsns[cal,4]);

          rc=dosubl('
             proc sql;
                create
                   table &out_dsn as
                select
                   r.*
                from
                   &in_dsn1 as l, &in_dsn2 as r
                where
                  l.&join_key = r.&join_key
             ;quit;
          ');

          nobs=symgetn('sqlobs');

          put cal=;

          if nobs eq 0 then do;
            status="Failed  ";
            message=catx(" ",put(nobs,8.),"obs after join");
            output;
            /*stop*/;
          end;
          else do;
            status="Success ";
            message=catx(" ",put(nobs,8.),"obs after join");
            output;
          end;

       end;

      stop;

     run;quit;

     *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;
      Put this statement in your program

       array dsns[2,4] $32 x1-x20
           (
             "sashelp.class","sashelp.classfit","class1","name"
             "sashelp.class","sashelp.classfit","class2","name"
             "sashelp.qtr111","sashelp.qtr1001","qtr1","t"
             "sashelp.prdsal3","sashelp.mdv","sal1","t"
             "sashelp.qtr111","sashelp.qtr1001","qtr2","t"

     *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

     data log(drop=x: cal rc);

       retain status message;

       * arguments for two "macro" calls;
       array dsns[5,4] $32 x1-x20
           (
             "sashelp.class","sashelp.classfit","class1","name"
            ,"sashelp.class","sashelp.classfit","class2","name"
            ,"sashelp.qtr111","sashelp.qtr1001","qtr1","t"
            ,"sashelp.prdsal3","sashelp.mdv","sal1","country"
            ,"sashelp.qtr111","sashelp.qtr1001","qtr2","t"
           );

       do cal=1 to 5;

         * need to make sure these are in the log;

         in_dsn1  = dsns[cal,1];
         in_dsn2  = dsns[cal,2];
         out_dsn  = dsns[cal,3];
         join_key = dsns[cal,4];

         * pass to dosubl;
         * note we could just pass the address of the dsns array;

         call symputx("in_dsn1 ",dsns[cal,1]);
         call symputx("in_dsn2 ",dsns[cal,2]);
         call symputx("out_dsn ",dsns[cal,3]);
         call symputx("join_key",dsns[cal,4]);

          rc=dosubl('
             proc sql;
                create
                   table &out_dsn as
                select
                   r.*
                from
                   &in_dsn1 as l, &in_dsn2 as r
                where
                  l.&join_key = r.&join_key
             ;quit;
          ');

          nobs=symgetn('sqlobs');

          put cal=;

          if nobs eq 0 then do;
            status="Failed  ";
            message=catx(" ",put(nobs,8.),"obs after join");
            output;
            /*stop*/;
          end;
          else do;
            status="Success ";
            message=catx(" ",put(nobs,8.),"obs after join");
            output;
          end;

       end;

      stop;

     run;quit;

    *_
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    ;


    1      data log(drop=x: cal rc);
    2        retain status message;
    3        * arguments for two "macro" calls;
    4        array dsns[5,4] $32 x1-x20
    5            (
    6              "sashelp.class","sashelp.classfit","class1","name"
    7             ,"sashelp.class","sashelp.classfit","class2","name"
    8             ,"sashelp.qtr111","sashelp.qtr1001","qtr1","t"
    9             ,"sashelp.prdsal3","sashelp.mdv","sal1","country"
    10            ,"sashelp.qtr111","sashelp.qtr1001","qtr2","t"
    11           );
    12       do cal=1 to 5;
    13         * need to make sure these are in the log;
    14         in_dsn1  = dsns[cal,1];
    15         in_dsn2  = dsns[cal,2];
    16         out_dsn  = dsns[cal,3];
    17         join_key = dsns[cal,4];
    18         * pass to dosubl;
    19         * note we could just pass the address of the dsns array;
    20         call symputx("in_dsn1 ",dsns[cal,1]);
    21         call symputx("in_dsn2 ",dsns[cal,2]);
    22         call symputx("out_dsn ",dsns[cal,3]);
    23         call symputx("join_key",dsns[cal,4]);
    24          rc=dosubl('
    25             proc sql;
    26                create
    27                   table &out_dsn as
    28                select
    29                   r.*
    30                from
    31                   &in_dsn1 as l, &in_dsn2 as r
    32                where
    33                  l.&join_key = r.&join_key
    34             ;quit;
    35          ');
    36          nobs=symgetn('sqlobs');
    37          put cal=;
    38          if nobs eq 0 then do;
    39            status="Failed  ";
    40            message=catx(" ",put(nobs,8.),"obs after join");
    41            output;
    42            /*stop*/;
    43          end;
    44          else do;
    45            status="Success ";
    46            message=catx(" ",put(nobs,8.),"obs after join");
    47            output;
    48          end;
    49       end;
    50      stop;
    51     run;

    NOTE: Table WORK.CLASS1 created, with 19 rows and 10 columns.

    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.02 seconds
          cpu time            0.01 seconds


    CAL=1
    NOTE: Table WORK.CLASS2 created, with 19 rows and 10 columns.

    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.01 seconds
          cpu time            0.01 seconds


    CAL=2
    NOTE: Table WORK.QTR1 created, with 57 rows and 90 columns.

    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.01 seconds
          cpu time            0.01 seconds


    CAL=3
    NOTE: Table WORK.SAL1 created, with 0 rows and 14 columns.

    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.02 seconds
          cpu time            0.01 seconds


    CAL=4
    NOTE: Table WORK.QTR2 created, with 57 rows and 90 columns.

    NOTE: PROCEDURE SQL used (Total process time):
          real time           0.00 seconds
          cpu time            0.01 seconds


    CAL=5
    NOTE: The data set WORK.LOG has 5 observations and 7 variables.
    NOTE: DATA statement used (Total process time):
          real time           0.45 seconds
          cpu time            0.37 seconds

    51  !      quit;

