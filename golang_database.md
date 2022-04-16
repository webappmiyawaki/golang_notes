go database/sql

https://sourjp.github.io/posts/go-db/

１．データベースを作成
２．"github.com/lib/pq"をインストール
３．接続


ユーザー名：postgres
パスワード：test

```sql
CREATE DATABASE golang_sample;

create TABEL legend(
    id serial primary key,
    legend_name varchar(255)
);
create TABLE player (
    id serial primary key,
    player_name varchar(255),
    legend_id integer REFERENCES legend(id)
);

SELECT * FROM legend;
SELECT * FROM player;
```


```go
package main

import (
	"database/sql"
	"errors"
	_ "github.com/lib/pq"
)

type Legend struct {
	Id          int
	Legend_name string
	Players     []Player
}

type Player struct {
	Id          int
	Player_name string
	Legend      *Legend
}

var Db *sql.DB

func init() {
	var err error
	Db, err = sql.Open("postgres", "user=postgres dbname=gosample password=test sslmode=disable")
	if err != nil {
		panic(err)
	}
}

func (legend *Legend) Create() (err error) {
	err = Db.QueryRow("insert into legend (legend_name) values ($1) returning id", legend.Legend_name).Scan(&legend.Id)
	return
}

func (player *Player) Create() (err error) {
	if player.Legend == nil {
		err = errors.New("レジェンドが設定されていません")
	}
	err = Db.QueryRow("insert into player (player_name, legend_id) values ($1, $2) returning id", player.Player_name, player.Legend.Id).Scan(&player.Id)
	return
}

func main() {
	legend := Legend{Legend_name: "ホライゾン"}
	legend.Create()

	player := Player{Player_name: "ippei", Legend: &legend}
	player.Create()
}

```