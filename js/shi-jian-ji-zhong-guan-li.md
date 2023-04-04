# 事件集中管理

* 事件集中管理（小爵老师的live分享）

```
var eventsMap = {
    'click .change-paper a': 'changepapaer',
    'click #cusGo': 'cusGo',
    'click #toHtml': 'toHtml',
    'click #close': 'close'
}

function scanEventsMap(maps, isOn) {
    var delegateEventSplitter = /^(\S+)\s*(.*)$/;
    var bind = isOn ? delegate : undelegate;
    for (var keys in maps) {
        if (maps.hasOwnProperty(keys)) {
            var matchs = keys.match(delegateEventSplitter);
            bind(matchs[1], matchs[2], maps[keys].bind(this));
        }
    }
}

function delegate(name, selector, func) {
    doc.on(name, selector, func);
}

function undelegate(name, selector, func) {
    doc.off(name, selector, func);
}

scanEventsMap(eventsMap)
```
