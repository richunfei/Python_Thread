import time
import winsound
from threading import Thread

cmd = None
available_cmd = ['exit', 'writelog']


def get_log_line(logPath, lineNumber):
    flog = open(logPath, 'r')
    lines = flog.readlines()
    flog.close()
    last_line = ''
    if len(lines) > 3:
        last_line = lines[lineNumber]
    return last_line


def alert_line(line, key='error', include=True, duration=1000, freq=440):
    error = False
    if include:
        if key in line:
            error = True
    else:
        if key not in line:
            error = True
    if error:
        print(line)
        winsound.Beep(freq, duration)


class Thread_CheckLog(Thread):
    minInterval = 2
    maxInterval = 20

    def __init__(self, logPath):
        super().__init__()
        self.logPath = logPath
        self.checkInterval = 5

    def run(self):
        _rest = -1
        while True:
            l = get_log_line(self.logPath, -1)
            alert_line(l, key='err')
            alert_line(l, key='nan')
            time.sleep(self.checkInterval)
            if _rest < 0:
                _rest = self.maxInterval
                winsound.PlaySound('SystemQuestion', winsound.SND_ALIAS)
                print(l)
            _rest -= self.checkInterval


class Thread_Write_Log(Thread):
    def __init__(self, logPath):
        super().__init__()
        self.logPath = logPath

    def run(self):
        logline = ''
        while not logline == 'exit':
            logline = input('(wr_log)>:')
            flog = open(self.logPath, 'a')
            flog.write('\n'+logline)
            flog.close()


def command():
    global cmd
    print(f"command list: {available_cmd}")
    while not cmd == 'exit':
        if cmd in available_cmd:
            if cmd == 'writelog':
                td_writelog = Thread_Write_Log(logP)
                td_writelog.start()
                td_writelog.join()
            cmd = input('cmd>:')
        else:
            cmd = input(f"Invalid command, available commands {available_cmd}]\n cmd>:")


if __name__ == '__main__':
    logP = 'd:/Work/log.txt'
    td_check_log = Thread_CheckLog(logPath=logP)
    td_check_log.start()
    td_cmd = Thread(target=command)
    td_cmd.start()
    td_cmd.join()

