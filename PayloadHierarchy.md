# Payload Hierarchy #

This is the payload hierarchy for the payloads in the fuzzdb module of pywebfuzz. This is just a simple reference for when you are instantiating these payload values in to variables. This piece of the module is rather ugly but is done this way to maintain some form of mapping to the fuzzdb project.

```
class attack_payloads:

     class all_attacks:
          all_attacks_unix
          all_attacks_win
          interesting_metacharacters

     class disclosure_directory:
          class generic:
               directory_indexing_generic

     class disclosure_localpaths:
          class microsoft:
          class unix:
               common_unix_httpd_log_locations

     class disclosure_source:
          source_disc_cmd_exec_traversal 
          source_disclosure_generic
          source_disclosure_microsoft

     class file_upload:
          alt_extensions_asp
          alt_extensions_coldfusion
          alt_extensions_jsp
          alt_extensions_perl
          alt_extensions_php
          file_ul_filter_bypass_commonly_writable_directories
          file_ul_filter_bypass_microsoft_asp_filetype_bf
          file_ul_filter_bypass_microsoft_asp
          file_ul_filter_bypass_ms_php
          file_ul_filter_bypass_x_platform_generic
          file_ul_filter_bypass_x_platform_php
          invalid_filenames_linux
          invalid_filenames_microsoft
          invalid_filesystem_chars_microsoft
          invalid_filesystem_chars_osx

```

Still a work in progress and not completed yet.