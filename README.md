# utl-using-sas-infile-input-to-parse-compex-datastep-strings-empty-files
Using sas infile input to parse compex datastep strings empty files
    %let pgm=utl-using-sas-infile-input-to-parse-compex-datastep-strings-empty-files;

    Using sas infile input to parse compex datastep strings empty files

       Problems
        1.  Slice a string into character numeric and dates values
        2.  Read only the diagonal letters in matrix of characters

    github
    https://tinyurl.com/5hnaemvy
    https://github.com/rogerjdeangelis/utl-using-sas-infile-input-to-parse-compex-datastep-strings-empty-files


    SEE BART'S SAS-L POST FOR THE ORIGINAL WORK
    https://listserv.uga.edu/scripts/wa-UGA.exe?A2=SAS-L;485edc4b.2403A&S=

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* 1.  SLICE A STRING INTO CHARACTER NUMERIC AND DATES VALUES                                                             */
    /*                                                                                                                        */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                            |                                  |                                                        */
    /*      INPUT                 |             PROCESS              |                      OUTPUT                            */
    /*                            |                                  |                                                        */
    /* =================          |  ==============================  |  ============================================          */
    /* *empty text file;          |                                  |                                                        */
    /* data _null_;               |  data slice;                     |          STR           CLASS     DTE     SIZE          */
    /*    file "c:/temp/null.txt" |                                  |                                                        */
    /*       lrecl=1 recfm=f;     |     infile "c:/temp/null.txt"    |   Math 04MAR2024 30    Math     23439     30           */
    /*    put '1A'x;  /*EOF*/     |                lrecl=80 recfm=f; |   Chem 04MAR2024 28    Chem     23439     28           */
    /* run;quit;                  |                                  |   Phys 04MAR2024 19    Phys     23439     19           */
    /*                            |     do until (dne);              |                                                        */
    /* data mixTyp;               |       set mixTyp end=dne;;       |                                                        */
    /*  length str $80;           |       input #1 @@;               |                                                        */
    /*  input str $80.;;          |       _infile_=str;              |                                                        */
    /* cards4;                    |       input                      |                                                        */
    /* Math 04MAR2024 30          |                class $4.         |                                                        */
    /* Chem 04MAR2024 28          |             +1 dte   date9.      |                                                        */
    /* Phys 04MAR2024 19          |             +1 size  2.    @@;   |                                                        */
    /* ;;;;                       |       output;                    |                                                        */
    /* run;quit;                  |     end;                         |                                                        */
    /*                            |                                  |                                                        */
    /*         STR                |   run;quit;                      |                                                        */
    /*                            |                                  |                                                        */
    /*  Math 04MAR2024 30         |                                  |                                                        */
    /*  Chem 04MAR2024 28         |                                  |                                                        */
    /*  Phys 04MAR2024 19         |                                  |                                                        */
    /*                            |                                  |                                                        */
    /**************************************************************************************************************************/


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* 2.  Read only the diagonal letters in matrix of characters                                                             */
    /*                                                                                                                        */
    /*------------------------------------------------------------------------------------------------------------------------*/
    /*                            |                                  |                                                        */
    /*      INPUT                 |             PROCESS              |                      OUTPUT                            */
    /*                            |                                  |                                                        */
    /* =================          |  ==============================  |  ============================================          */
    /*                            |                                  |                                                        */
    /* data _null_;               |   data slice;                    |         STR  POSITION  DIAGONAL                        */
    /*    file "c:/temp/null.txt" |                                  |                                                        */
    /*       lrecl=1 recfm=f;     |     infile "c:/temp/null.txt"    |       19234     1         9                            */
    /*    put '1A'x;  /*EOF*/     |       lrecl=80 recfm=f ;         |       21934     2         9                            */
    /* run;quit;                  |     do until (dne);              |       31294     3         9                            */
    /*                            |       set have end=dne;;         |       41239     4         9                            */
    /* data have;                 |        input #1 @@;              |                                                        */
    /* input str $;               |        _infile_=str;             |                                                        */
    /* cards4;                    |        input idx 1. @ ;          |                                                        */
    /* 19234                      |          input @(idx+1)          |                                                        */
    /* 21934                      |            diagonal 1. @;        |                                                        */
    /* 31294                      |        output;                   |                                                        */
    /* 41239                      |      end;                        |                                                        */
    /* ;;;;                       |                                  |                                                        */
    /* run;quit;                  |    run;quit;                     |                                                        */
    /*                            |                                  |                                                        */
    /**************************************************************************************************************************/

    /*                   _               _              _   _
    (_)_ __  _ __  _   _| |_   _ __ ___ (_)_  _____  __| | | |_ _   _ _ __   ___  ___
    | | `_ \| `_ \| | | | __| | `_ ` _ \| \ \/ / _ \/ _` | | __| | | | `_ \ / _ \/ __|
    | | | | | |_) | |_| | |_  | | | | | | |>  <  __/ (_| | | |_| |_| | |_) |  __/\__ \
    |_|_| |_| .__/ \__,_|\__| |_| |_| |_|_/_/\_\___|\__,_|  \__|\__, | .__/ \___||___/
            |_|                                                 |___/|_|
    */

    /*---- put this in your autoexec  (empty text file)                      ----*/
    data _null_;
       file "c:/temp/null.txt"
          lrecl=1 recfm=f;
       put '1A'x;  /*EOF*/
    run;quit;

     data mixTyp;
      length str $80;
      input str $80.;;
     cards4;
     Math 04MAR2024 30
     Chem 04MAR2024 28
     Phys 04MAR2024 19
     ;;;;
     run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*         STR                                                                                                            */
    /*                                                                                                                        */
    /*  Math 04MAR2024 30                                                                                                     */
    /*  Chem 04MAR2024 28                                                                                                     */
    /*  Phys 04MAR2024 19                                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                             _              _   _
     _ __  _ __ ___   ___ ___  ___ ___   _ __ ___ (_)_  _____  __| | | |_ _   _ _ __   ___  ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __| | `_ ` _ \| \ \/ / _ \/ _` | | __| | | | `_ \ / _ \/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \ | | | | | | |>  <  __/ (_| | | |_| |_| | |_) |  __/\__ \
    | .__/|_|  \___/ \___\___||___/___/ |_| |_| |_|_/_/\_\___|\__,_|  \__|\__, | .__/ \___||___/
    |_|                                                                   |___/|_|
    */

    data slice;

       infile "c:/temp/null.txt"
                  lrecl=80 recfm=f;

       do until (dne);
         set mixTyp end=dne;;
         input #1 @@;
         _infile_=str;
         input
               class $4.
               +1 dte date9.
               +1 size 2.   @@;
         output;
       end;

     run;quit;

     /**************************************************************************************************************************/
     /*                                                                                                                        */
     /* 1.  SLICE A STRING INTO CHARACTER NUMERIC AND DATES VALUES                                                             */
     /*                                                                                                                        */
     /*                     OUTPUT                                                                                             */
     /*                                                                                                                        */
     /* ============================================                                                                           */
     /*                                                                                                                        */
     /*         STR           CLASS     DTE     SIZE                                                                           */
     /*                                                                                                                        */
     /*  Math 04MAR2024 30    Math     23439     30                                                                            */
     /*  Chem 04MAR2024 28    Chem     23439     28                                                                            */
     /*  Phys 04MAR2024 19    Phys     23439     19                                                                            */
     /*                                                                                                                        */
     /**************************************************************************************************************************/

    /*                   _         _ _                               _
    (_)_ __  _ __  _   _| |_    __| (_) __ _  __ _  ___  _ __   __ _| |
    | | `_ \| `_ \| | | | __|  / _` | |/ _` |/ _` |/ _ \| `_ \ / _` | |
    | | | | | |_) | |_| | |_  | (_| | | (_| | (_| | (_) | | | | (_| | |
    |_|_| |_| .__/ \__,_|\__|  \__,_|_|\__,_|\__, |\___/|_| |_|\__,_|_|
            |_|                              |___/
    */

    data _null_;
       file "c:/temp/null.txt"
          lrecl=1 recfm=f;
       put '1A'x;  /*EOF*/
    run;quit;

    data have;
    input str $;
    cards4;
    19234
    21934                      data have;
    31294                      input str $;
    41239                      cards4;
    ;;;;                       11234
    run;quit;                  21234
    31234
    41234
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs     STR         Note 9 on the diagonal ( we only read the diagonal)                                               */
    /*                                                                                                                        */
    /*   1     19234            1 9 2 3 4          ( position is arbitrart can even @char variable)                           */
    /*   2     21934            2 1 9 3 4                                                                                     */
    /*   3     31294            3 1 2 9 4                                                                                     */
    /*   4     41239            4 1 2 3 9                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                       _ _                               _
     _ __  _ __ ___   ___ ___  ___ ___    __| (_) __ _  __ _  ___  _ __   __ _| |
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|  / _` | |/ _` |/ _` |/ _ \| `_ \ / _` | |
    | |_) | | | (_) | (_|  __/\__ \__ \ | (_| | | (_| | (_| | (_) | | | | (_| | |
    | .__/|_|  \___/ \___\___||___/___/  \__,_|_|\__,_|\__, |\___/|_| |_|\__,_|_|
    |_|                                                |___/
    */

     data slice ;

       infile "c:/temp/null.txt"
         lrecl=80 recfm=f ;
       do until (dne);
         set have end=dne;;
          input #1 @@;
          _infile_=str;
          input idx 1. @ ;
            * note you can manipulate index ;
            input @(idx+1)
              diagonal 1. @;
          output;
        end;

      run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  STR     IDX    DIAGONAL   Note 9 on the diagonal                                                                      */
    /*                                                                                                                        */
    /* 19234     1         9          1 9 2 3 4                                                                               */
    /* 21934     2         9          2 1 9 3 4                                                                               */
    /* 31294     3         9          3 1 2 9 4                                                                               */
    /* 41239     4         9          4 1 2 3 9                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    Empty files
    https://github.com/rogerjdeangelis/utl-check-to-determine-if-a-file-is-empty
    https://github.com/rogerjdeangelis/utl-check-to-determine-if-a-file-is-empty-at-datastep-compilation-phase
    https://github.com/rogerjdeangelis/utl-displaying-empty-rows-and-colums-in-a-crosstab-when-the-those-rows-and-columns-are-not-present-i
    https://github.com/rogerjdeangelis/utl-do-NOT-create-text-file-if-input-table-is-empty
    https://github.com/rogerjdeangelis/utl-extract-n-last-non-empty-cells-of-each-row-using-sas
    https://github.com/rogerjdeangelis/utl-processing-empty-files-along-with-files-containing-text
    https://github.com/rogerjdeangelis/utl-skipping-empty-files-when-importing
    https://github.com/rogerjdeangelis/utl-the-four-types-of-empty-sas-tables
    https://github.com/rogerjdeangelis/utl_check_if_a_file_is_empty
    https://github.com/rogerjdeangelis/utl_how_to_carry_forwand_the_last_non_empty_value_of_a_sas_wps_array
    https://github.com/rogerjdeangelis/utl_programmatically_remove_empty_tables_from_a_merge_or_sql_join

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
