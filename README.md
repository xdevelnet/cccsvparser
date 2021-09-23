# CSV Parser
C library for parsing CSV files

This library is a copy of http://sourceforge.net/projects/cccsvparser/
I've uploaded it here in order to continue development as the original appears to be inactive.

Example usage:

    #include <stdio.h>

    #include "csvparser.h"

    int main() {
        int i =  0;
        //                                   file, delimiter, first_line_is_header?
        CsvParser *csvparser = CsvParser_new("Book1.csv", ",", 1);
        CsvRow *header;
        CsvRow *row;

        header = CsvParser_getHeader(csvparser);
        if (header == NULL) {
            printf("%s\n", CsvParser_getErrorMessage(csvparser));
            return 1;
        }
        char **headerFields = CsvParser_getFields(header);
        for (i = 0 ; i < CsvParser_getNumFields(header) ; i++) {
            printf("TITLE: %s\n", headerFields[i]);
        }
        // CsvParser_destroy_row(header); -> causes error in current version
        while ((row = CsvParser_getRow(csvparser)) ) {
            printf("NEW LINE:\n");
            char **rowFields = CsvParser_getFields(row);
            for (i = 0 ; i < CsvParser_getNumFields(row) ; i++) {
                printf("FIELD: %s\n", rowFields[i]);
            }
            CsvParser_destroy_row(row);
        }
        CsvParser_destroy(csvparser);
        return 0;
    }