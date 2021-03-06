<template>
  <div class="page">
    <el-card class="card">
      <h1 style="padding-bottom:1rem">History:</h1>
      <div class="history desktop">
        <div v-if="history.error" style="padding-top: 1rem">
          <h4 style="color: red">{{ history.error.message }}</h4>
          <br>
          <nuxt-link to="/getwallet">
            <el-button type="success" size="medium">Get Wallet</el-button>
          </nuxt-link>
        </div>
        <el-table v-if="!history.error" :data="history" style="max-width: 100%" max-height="500" 
          :header-row-class-name="tableRowClassName"
          :row-class-name="tableRowClassName">
          <el-table-column label="Sender">
            <template slot-scope="props">
              <span>{{props.row.sender ? sliceString(props.row.sender) : null}}</span>
            </template>
          </el-table-column>
          <el-table-column label="Amount">
             <template slot-scope="props">
              <span>{{props.row.amount ? (props.row.amount / Math.pow(10, 8)) : null}}</span>
            </template>
          </el-table-column>
          <el-table-column prop="fee" label="Fee">
            <template slot-scope="props">
              <span>{{props.row.fee ? (props.row.fee / Math.pow(10, 8)) : null}}</span>
            </template>
          </el-table-column>
          <el-table-column label="Recipient">
            <template slot-scope="props">
              <span>{{props.row.recipient ? sliceString(props.row.recipient) : null}}</span>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="mobile">
        <div v-if="history.error" style="padding-top: 1rem">
          <h4 style="color: red">{{ history.error.message }}</h4>
          <br>
          <nuxt-link to="/getwallet">
            <el-button type="success" size="medium">Get Wallet</el-button>
          </nuxt-link>
        </div>
        <el-table
          v-if="!history.error"
          :data="history"
          style="width: 100%"
          max-height="550" 
          :header-row-class-name="tableRowClassName"
          :row-class-name="tableRowClassName">
          <el-table-column type="expand">
            <template slot-scope="props">
              <div>
                <el-tag>Amount: </el-tag>
                <span style="color:#3076bf">{{ props.row.amount ? (props.row.amount / Math.pow(10, 8)) : null }}</span>
              </div>
              <div style="padding-top: 5px">
                <el-tag>Fee: </el-tag>
                <span style="color:#3076bf">{{ props.row.fee ? (props.row.fee / Math.pow(10, 8)) : null }}</span>
              </div>
              <div style="padding-top: 5px">
                <el-tag>Recipient: </el-tag>
                <span style="color:#3076bf; word-wrap: break-word">{{ props.row.recipient ? sliceString(props.row.recipient) : null}}</span>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            label="Sender">
              <template slot-scope="props">
                <span>{{props.row.sender ? sliceString(props.row.sender) : null}}</span>
              </template>
          </el-table-column>
        </el-table>
      </div>
    </el-card>
  </div>
</template>
 
<script>
import axios from "axios";
import Cookie from "js-cookie";

export default {
  middleware: ["auth"],
  asyncData ({req, res, error, redirect}) {
    let token;
    if (process.server) {
      const jwtCookie = req.headers.cookie
        .split(";")
        .find(c => c.trim().startsWith("jwt="));
      if (!jwtCookie) {
        return;
      }
      token = jwtCookie.split("=")[1];
    }
    if (process.client) {
      token = Cookie.get("jwt");
    }
    const config = {
      headers: {
        Authorization: "Bearer " + token
      }
    };
    return axios.get(process.env.KEUrl + "/trx-history", config)
      .then((res) => {
        return { history: res.data }
      })
      .catch((e) => {
        redirect({
          name: 'login'
        })
      })
  },
  methods: {
    tableRowClassName({row, rowIndex}) {
      return 'warning-row';
    },
    sliceString(str) {
      const firstChar = str.slice(0,4);
      const lastChar = str.slice(-3);
      return firstChar + '.........' + lastChar;
    }
  },
};
</script>

<style scoped>
h1, h4 {
  color: white;
}
/* // RESPONSIVE */
/* //SmartPhone */
@media only screen and (max-width: 500px) {
  .desktop {
    display: none;
  }
}
/* //Tablet */
@media only screen and (min-width: 501px) and (max-width: 767px) {
  .desktop {
    display: none;
  }
}
/* //Normal */
@media only screen and (min-width: 768px) and (max-width: 1199px){
  .mobile {
    display: none;
  }
}
/* Large monitor */
@media only screen and (min-width: 1200px) and (max-width: 1919px) {
  .mobile {
    display: none;
  }       
}
/* //Landscape */
@media only screen and (max-height: 500px) {
  .mobile {
    display: none;
  }    
}
/* Widescreen */
@media only screen and (min-width: 1920px) {
  .mobile {
    display: none;
  }   
}
</style>