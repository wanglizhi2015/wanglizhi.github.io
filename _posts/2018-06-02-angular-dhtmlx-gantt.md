---
title: Angular集成dhtmlx-gantt甘特图
tags: angular
layout: post
---

### 安装dhtmlx-gantt甘特图依赖
```js
npm install dhtmlx-gantt --save
```

### 新增甘特图weekly-plan-management.componet.ts组件如下：
```js
import {Component, ElementRef, OnInit, ViewChild} from '@angular/core';
import "dhtmlx-gantt";


import {TaskServiceService} from "./task-service.service";
import {LinkServiceService} from "./link-service.service";

@Component({
  selector: 'weekly-plan-management',
  styles: [
      `
      :host {
        display: block;
        height: 600px;
        position: relative;
        width: 100%;
      }
    `],
  providers: [TaskServiceService, LinkServiceService],
  template: "<div #gantt_here style='width: 100%; height: 100%;'></div>",
})

export class WeeklyPlanManagementComponent implements OnInit {

  constructor(
      private taskService: TaskServiceService, 
      private linkService: LinkServiceService
      ) {
  }

  @ViewChild("gantt_here") ganttContainer: ElementRef;

  ngOnInit() {
    gantt.config.xml_date = "%Y-%m-%d %H:%i";

    gantt.init(this.ganttContainer.nativeElement);

    Promise.all([this.taskService.get(), this.linkService.get()])
      .then(([data, links]) => {
        gantt.parse({data, links});
    });
  }

}

```
### Link.ts代码如下：
```js
export class Link {
  id: number;
  source: number;
  target: number;
  type: string;
}
```

### Task.ts代码如下：
```js
export class Task {
  id: number;
  start_date: string;
  text: string;
  duration: number;
  progress: number;
}
```

### task-service.service.ts代码如下：
```js
import { Injectable } from '@angular/core';
import {Task} from "./Task";
@Injectable({
  providedIn: 'root'
})
export class TaskServiceService {
  get(): Promise<Task[]>{
    return Promise.resolve([
      {id: 1, text: "Task #1", start_date: "2017-04", duration: 3, progress: 0.6},
      {id: 2, text: "Task #2", start_date: "2017-05", duration: 3, progress: 0.4}
    ]);
  }
}
```

### link-service.service.ts代码如下：
```js
import { Injectable } from '@angular/core';
import {Link} from "./link";

@Injectable({
  providedIn: 'root'
})
export class LinkServiceService {

  get(): Promise<Link[]> {
    return Promise.resolve([
      {id: 1, source: 1, target: 2, type: "0"}
    ]);
  }
}
```

### src/style.less代码如下：
```js
@import '../node_modules/dhtmlx-gantt/codebase/dhtmlxgantt.css';
```
