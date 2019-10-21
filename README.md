### portmidi
---
https://github.com/philandstuff/portmidi

https://github.com/rakyll/portmidi

```c
// pm_test/mm.c

#include "stdlib.h"
#incude "ctype.h"
#include "string.h"
#include "stdio.h"
#include "porttime.h"
#include "portmidi.h"

#define STRING_MAX 80

#define MIDI_CODE_MASK 0xf0
#define MIDI_CHN_MASK 0x0f
#define MIDI_OFF_NOTE 0x80



int debug = false;
PmStream *midi_in;
boolean active = false;

uint32_t filter = 0;

uint32_t clockcount = 0;
uint32_t actsensecount = 0;
uint32_t notescount = 0;
uint32_t notestotal = 0;

char val_format[] = " Val %d\n";

extern int abort_flag;

private void mmexit(int code);
private void output(pmMessage data);
private int put_pitch(int p);
private void showhelp();
private void showsbutes(PmMessage data, int len, boolean newline);
private void showstatus(boolean flag);
private void doascii(char c);
private int get_number(char *prompt);

int get_number(char *prompt)
{
  char line[STRING_MAX];
  int n = 0, i;
  printf(prompt);
  while (n != 1) {
    n = scanf("%d", &i);
    fgets(line, STRING_MAX, stdin);
  }
  return i;
}

void receive_poll(PtTimestamp timestamp, void *userData)
{
  PmEvent event;
  int count;
  if (!active) return;
  while ((count = Pm_Read(midi_in, &event, 1))) {
    if ((count == 1) output(event.message);
    else printf(Pm_GetErrorText(count)));
  }
}




```

```go




```

```
```


