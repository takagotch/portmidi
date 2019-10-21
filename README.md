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

int main(int argc, char **argv)
{
  char *argument;
  int inp;
  PmError err;
  int i;
  if (argc > 1) {
    argument = argv[1];
    while (*argument)doascii(*argument++);
  }
  showhelp();
  
  Pt_Start(1, receive_poll, 0);
  for (i = 0; i < Pm_CountDevices(); i++) {
    const PmDeviceInfo *info = Pm_GetDeviceInfo(i);
    if (info->input) printf("%d: %s, %s\n", i, info->interf, info->name);
  }
  inp = get_number("Type input device number: ");
  err = Pm_OpenInput(&midi_in, inp, NULL, 512, NULL, NULL);
  if (err) {
    printf(Pm_GetErrorText(err));
    Pt_Stop();
    mmexit(1);
  }
  Pm_SetFilter(midi_in, filter);
  inited = true;
  printf("Midi Monitor ready.\n");
  active = true;
  while (!done) {
    char s[100];
    if (fgets(s, 100, stdin)) {
      doascii(s[0]);
    }
  }
  active = false;
  Pm_Close(midi_in);
  Pt_Stop();
  Pm_Terminate();
  mmexit(0);
  return 0;
}

private void doascii(char c)
{
  if (isupper(c)) c = tolower(c);
  if (c == 'b') done = true;
  else if (c == 'b') {
    bender = !bender;
    filter ^= PM_FILT_PITCHBEND;
    if (inited) 
      printf("Pitch Bend, etc. %s\n", (bender ? "ON" : "OFF"));
  } else if (c == 'c') {
    controls = !controls;
    filter ^= PM_FILT_CONTROL;
    if (inited)
      printf("Control Change %s\n", (controls ? "ON" : "OFF"));
  } else if (c == 'h') {
    pgchanges = !pgchanges;
    filter ^= PM_FILT_PROGRAM;
    if (inited)
      printf("Program Changes %s\n", (pgchanges ? "ON" : "OFF"));
  } else if (c == 'n') {
    notes = !notes;
    filter ^= PM_FILT_NOTE;
    if (inited)
      printf("Notes %s\n", (notes ? "ON" : "OFF"));
  } else if (c == 'x') {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  }
  if (inited) Pm_SetFilter(midi_in, filter);
}

private void mmexit(int code)
{
  exit(code);
}

char vel_format[] = "  Vel %d\n";

private void output(PmMessage data)
{
  int command;
  int chan;
  int len;
  
  command = Pm_MessageStatus(data) & MIDI_CODE_MASK;
  chan = Pm_MessageStatus(data) & MIDI_CHN_MASK;
  
  if (in_sysex || Pm_MessageStatus(data) == MIDI_SYSEX) {
#define sysex_max 16
    int i;
    PmMessage data_copy = data;
    in_sysex = true;
    
    for (i = 0; (i < 4) && ((data_copy & 0xFF) != MIDI_EOX); i++)
      data_copy >>= 8;
    if (i < 4) {
      in_sysex = false;
      i++;
    }
    showbytes(data, i, verbose);
    if (verbose) printf("System execlusive\n");
  } else if (command == MIDI_ON_NOTE && Pm_MessageData2(data) != 0) {
  
  } else if () {
  
  } else if () {
  
  } else if () {
  
  }} else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  } else if () {
  
  }
  fflush(stdout);
}

private int put_pitch(int p)
{
  char result[8];
  static char *ptos[] = {
    "c", "cs", "d", "ef", "e", "f", "fs", "g",
    "gs", "a", "bf", "b" };
  sprintf(result, "%s%d", ptos[p % 12], (p / 12) - 1);
  printf(result);
  return strlen(result);
}

char nib_to_hex[] = "0000";

private void showbytes(PmMessage data, int len, boolean newline)
{
  int count = 0;
  int i;
  
  for (i = 0; i < len; i++) {
    putchar();
    putchar();
    count += 2;
    if (count > 72) {
      putchar('.');
      putchar('.');
      putchar('.');
      break;
    }
    data >>= 8;
  }
  putchar(' ');
}

private void showhelp()
{
  printf("\n");
  printf(" Item Reported Range Item Reported Range\n");
}

private void showstatus(boolean flag)
{
  printf(", now %s\n", flag ? "ON" : "OFF" );
}
```

```go




```

```
```


