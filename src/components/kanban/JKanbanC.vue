<template>
  <div>
    <button v-on:click="count++">You clicked me {{ count }} times.</button>
    <div id="myKanban"></div>
  </div>
</template>

<script>
import JKanban from "jkanban";


// require("jkanban")
// require("jkanban/jkanban.css")

export default {
    name: "kanban"
    ,methods: {
      getPipeData: function(){
          this.pipeData = [
          {
            id: "_todo",
            title: "To Do (Can drop item only in working)",
            class: "info,good",
            dragTo: ["_working"],
            item: [
              {
                id: "_test_delete",
                title: "Try drag this (Look the console)",
                drag: function(el, source) {
                  console.log("START DRAG: " + el.dataset.eid);
                },
                dragend: function(el) {
                  console.log("END DRAG: " + el.dataset.eid);
                },
                drop: function(el) {
                  console.log("DROPPED: " + el.dataset.eid);
                }
              },
              {
                title: "Try Click This!",
                click: function(el) {
                  alert("click");
                },
                class: ["peppe", "bello"]
              }
            ]
          },
          {
            id: "_working",
            title: "Working (Try drag me too)",
            class: "warning",
            item: [
              {
                title: "Do Something!"
              },
              {
                title: "Run?"
              }
            ]
          },
          {
            id: "_done",
            title: "Done (Can drop item only in working)",
            class: "success",
            dragTo: ["_working"],
            item: [
              {
                title: "All right"
              },
              {
                title: "Ok!"
              }
            ]
          }
        ]

      }
  }
  ,data: function () {
    return {
      count: 0,
      pipeData: []
    }
  },
  mounted() {
    var KanbanTest = new jKanban({
        element: "#myKanban",
        gutter: "10px",
        widthBoard: "450px",
        itemHandleOptions:{
            enabled: true,
        },
        click: function(el) {
            console.log("Trigger on all items click!");
        },
        dropEl: function(el, target, source, sibling){
            console.log(target.parentElement.getAttribute('data-id'));
            console.log(el, target, source, sibling)
        },
        buttonClick: function(el, boardId) {
            console.log(el);
            console.log(boardId);
            // create a form to enter element
            var formItem = document.createElement("form");
            formItem.setAttribute("class", "itemform");
            formItem.innerHTML =
            '<div class="form-group"><textarea class="form-control" rows="2" autofocus></textarea></div><div class="form-group"><button type="submit" class="btn btn-primary btn-xs pull-right">Submit</button><button type="button" id="CancelBtn" class="btn btn-default btn-xs pull-right">Cancel</button></div>';

            KanbanTest.addForm(boardId, formItem);
            formItem.addEventListener("submit", function(e) {
            e.preventDefault();
            var text = e.target[0].value;
            KanbanTest.addElement(boardId, {
                title: text
            });
            formItem.parentNode.removeChild(formItem);
            });
            document.getElementById("CancelBtn").onclick = function() {
            formItem.parentNode.removeChild(formItem);
            };
        },
        addItemButton: true,
        boards: []
    });
    this.getPipeData();
    console.log('my pipe data::', this.pipeData );
    KanbanTest.addBoards(this.pipeData);

  }
 
};
</script>

<style>
    @import url('~jkanban/jkanban.css');
</style>